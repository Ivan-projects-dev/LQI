#Quantum #Beginner #Debugging #Experience
How to actually figure out what is wrong with a quantum circuit. Per-platform techniques.

## The Core Problem

You cannot watch a quantum circuit run. Measurement destroys the state. If you add a measurement mid-circuit to check intermediate state, you collapse the superposition and change the computation. This makes quantum debugging fundamentally different from classical debugging.

The strategy is: **reconstruct the state from statistics, not direct observation.**

## Universal Rules Before Platform-Specific Techniques

1. **Test on simulator first.** If it fails on simulator, it is a circuit logic bug. If it passes simulator but fails on hardware, it is a noise/transpilation issue. Never skip this split.

2. **Use known circuits as sanity checks.** Before debugging your own circuit, run a Bell state. If Bell state gives wrong results, your environment is broken — not your circuit.

3. **Compare shot counts, not single shots.** One weird outcome proves nothing. Patterns in $1000$+ shots reveal the truth.

4. **Reduce circuit depth.** Remove gates one at a time until the circuit starts working. The last removed gate is the problem.

## IBM Quantum / Qiskit

### Statevector Simulation (Before Measurement)

To inspect quantum state mid-circuit without collapsing it, use `save_statevector()` in a simulator context:

```python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator

qc = QuantumCircuit(2)
qc.h(0)
qc.save_statevector()   # capture state here
qc.cx(0, 1)
qc.save_statevector()   # capture state after CNOT too

sim = AerSimulator(method='statevector')
result = sim.run(qc).result()

sv0 = result.data(0)['statevector']
print("After H:", sv0)
```

This only works in simulation. The `save_statevector` instruction has no hardware equivalent.

### Checking Circuit Structure

```python
print(qc.draw())           # ASCII diagram
print(qc.count_ops())      # {'h': 1, 'cx': 1, 'measure': 2}
print(qc.depth())          # critical path length
print(qc.num_qubits)
```

After transpilation, check what changed:

```python
from qiskit import transpile
tqc = transpile(qc, backend, optimization_level=3)
print(tqc.count_ops())     # native gates: ecr, rz, sx, x
print(tqc.depth())         # usually increases after routing
```

If `tqc.depth()` is much larger than `qc.depth()`, the routing inserted many SWAP gates. This is expensive — each SWAP is $3$ CNOTs on real hardware.

### Noise Simulation Without Real Hardware

To simulate hardware-realistic errors offline:

```python
from qiskit_aer import AerSimulator
from qiskit_ibm_runtime.fake_provider import FakeSherbrooke

noisy_sim = AerSimulator.from_backend(FakeSherbrooke())
job = noisy_sim.run(tqc, shots=1024)
result = job.result()
counts = result.get_counts()
print(counts)  # shows realistic error rates without queue wait
```

`FakeSherbrooke` (127 qubits) and `FakeManilaV2` (5 qubits) are good choices. This is the fastest debugging loop: simulate noise locally before spending QPU time.

### Reading Real Hardware Calibration

```python
props = backend.properties()
for gate in props.gates:
    if gate.gate == 'cx':
        for param in gate.parameters:
            if param.name == 'gate_error':
                print(f"CX({gate.qubits}): error={param.value:.4f}")
```

This tells you which qubit pairs have the lowest CNOT error right now. If you have flexibility in which qubits to use, pick the best ones. Calibration data refreshes every few hours.

## Azure Quantum / Q#

### DumpMachine — Inspect Full State

Q#'s most powerful debugging tool: outputs the complete statevector at the moment called.

```qsharp
import Std.Diagnostics.*;

operation DebugMyCircuit() : Unit {
    use q = Qubit[2];
    H(q[0]);
    Message("After H on q[0]:");
    DumpMachine();   // prints full statevector to Debug Console
    CNOT(q[0], q[1]);
    Message("After CNOT:");
    DumpMachine();
    ResetAll(q);
}
```

Output appears in the **Debug Console** panel in VS Code, not the Terminal. The format shows basis states and amplitudes:

```
# wave function for qubits with ids (least to most significant): 0;1
∣0❭: 0.707107 + 0.000000 i == ******* [ 0.500000 ]
∣2❭: 0.707107 + 0.000000 i == ******* [ 0.500000 ]
```

State $|0angle$ = `00`, state $|2angle$ = `10` in binary (little-endian: q[0] is LSB).

### DumpRegister — Inspect a Subset

```qsharp
DumpRegister(q[0..1]);  // only these qubits
```

Useful when you have ancilla qubits and want to ignore them.

### Checking Qubit State Before Release

The quantum simulator will throw a runtime error if you release (`use` block exits) a qubit that is not in $|0angle$. This is intentional — it enforces correct uncomputation.

```
Qubit in invalid state. Expected: Zero, Actual: One
```

Fix: add `Reset(q)` or `ResetAll(qs)` before the end of the `use` block. Better: use `within/apply` — it handles uncomputation automatically.

### Testing Operations with Known Inputs

```qsharp
operation TestMyOracle() : Unit {
    use (reg, target) = (Qubit[3], Qubit());
    // prepare a known input
    X(reg[0]); X(reg[2]);  // |101⟩ = 5 in binary
    
    MyOracle(reg, target);
    
    DumpMachine();  // check target was flipped correctly
    ResetAll(reg + [target]);
}
```

Run this for each case you care about. Q# has no built-in unit test framework but you can loop over known inputs and use `M()` to assert the output.

## PennyLane

### Intermediate State with qml.state()

```python
import pennylane as qml
import numpy as np

dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def debug_circuit():
    qml.Hadamard(wires=0)
    # check state here
    return qml.state()   # returns full statevector

print(debug_circuit())
# [0.707+0j, 0, 0.707+0j, 0]  = (|00⟩ + |10⟩)/√2
```

`qml.state()` returns the full statevector as a NumPy array. You can only call this on `default.qubit` — not on shot-based or hardware devices.

### Drawing the Circuit

```python
@qml.qnode(dev)
def my_circuit():
    qml.Hadamard(wires=0)
    qml.CNOT(wires=[0, 1])
    return qml.expval(qml.PauliZ(0))

print(qml.draw(my_circuit)())
```

Output:
```
0: ──H──╭●──┤ <Z>
1: ─────╰X──┤    
```

### Debugging Gradients

If your gradient is zero when it should not be:

```python
@qml.qnode(dev, diff_method="parameter-shift")
def circuit(theta):
    qml.RX(theta, wires=0)
    return qml.expval(qml.PauliZ(0))

theta = np.array(0.5, requires_grad=True)
grad = qml.grad(circuit)(theta)
print(grad)  # should be ~ -0.479
```

Common causes of zero gradient:
1. `theta` is a plain Python float, not `np.array(..., requires_grad=True)`
2. Using `import numpy as np` instead of PennyLane's `np` — `from pennylane import numpy as np`
3. The expectation value is $0$ at the current parameters and the gradient is genuinely zero at that point — shift slightly and try again

## Amazon Braket

### LocalSimulator for Free, Instant Testing

```python
from braket.devices import LocalSimulator

device = LocalSimulator()
task = device.run(circuit, shots=1000)
result = task.result()
print(result.measurement_counts)
```

The `LocalSimulator` runs in your process with no API calls, no S3 buckets, no queue. It supports up to $~25$ qubits before it becomes slow. Always debug here.

### Inspecting Circuit Structure

```python
print(circuit)
# prints ASCII diagram of the circuit

print(circuit.depth)        # gate depth
print(circuit.qubit_count)  # number of qubits
```

### Reading QPU Results

On a real QPU job:

```python
task = device.run(circuit, s3_destination_folder=("my-bucket", "prefix"), shots=1000)
task_id = task.id
print("Task ID:", task_id)

# retrieve later
from braket.aws import AwsQuantumTask
task = AwsQuantumTask(arn=task_id)
result = task.result()   # blocks until complete
print(result.measurement_counts)
```

## D-Wave

### Checking Chain Breaks

```python
sampleset = sampler.sample_qubo(Q, num_reads=100, chain_strength=2.0)

# fraction of reads with broken chains
print("Chain break fraction:", sampleset.first.chain_break_fraction)
```

A chain break fraction above $0.05$ ($5\%$) means your chain strength is too low. The physical qubits representing a logical variable disagree — the result is garbage.

Fix: increase `chain_strength`. A rough starting value is the maximum absolute coefficient in your QUBO matrix.

```python
max_coeff = max(abs(v) for v in Q.values())
chain_strength = max_coeff * 1.5
```

### Visualizing the Embedding

```python
from dwave.system import DWaveSampler, EmbeddingComposite
from minorminer import find_embedding

sampler = DWaveSampler()
source_edgelist = list(Q.keys())
target_edgelist = sampler.edgelist
embedding = find_embedding(source_edgelist, target_edgelist)

print("Longest chain:", max(len(v) for v in embedding.values()))
print("Number of chains:", len(embedding))
```

Long chains (length $> 5$) are fragile under noise. Reformulate your problem to reduce connectivity, or use `LeapHybridSampler` which handles dense problems natively.

## Sources
- [Qiskit: debugging circuits](https://docs.quantum.ibm.com/guides/debug-qiskit)
- [Q# DumpMachine documentation](https://learn.microsoft.com/en-us/qsharp/api/qsharp-core/microsoft.quantum.diagnostics/dumpmachine)
- [PennyLane: inspecting circuits](https://docs.pennylane.ai/en/stable/introduction/inspecting_circuits.html)
- [Amazon Braket: local simulators](https://docs.aws.amazon.com/braket/latest/developerguide/braket-devices.html)
- [D-Wave: chain breaks](https://docs.dwavesys.com/docs/latest/c_qpu_annealing.html)

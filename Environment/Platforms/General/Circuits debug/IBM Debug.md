#SoftDev #Python

Statevector Simulation (Before Measurement). To inspect [[Quantum state]] mid-circuit without collapsing it, use `save_statevector()` in simulator context:
```python
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator

qc = QuantumCircuit(2)
qc.h(0)
qc.save_statevector() # capture state here
qc.cx(0, 1)
qc.save_statevector() # capture state after CNOT too
sim = AerSimulator(method='statevector')
result = sim.run(qc).result()
sv0 = result.data(0)['statevector']
print("After H:", sv0)
```
This only works in simulation. The `save_statevector` instruction has no hardware equivalent.
### Checking Circuit Structure
```python
print(qc.draw()) # ASCII diagram
print(qc.count_ops()) # {'h': 1, 'cx': 1, 'measure': 2}
print(qc.depth()) # critical path length
print(qc.num_qubits)
```
After transpilation, check what changed:
```python
from qiskit import transpile
tqc = transpile(qc, backend, optimization_level=3)
print(tqc.count_ops()) # native gates: ecr, rz, sx, x
print(tqc.depth()) # usually increases after routing
```
If `tqc.depth()` is much larger than `qc.depth()`, routing inserted many [[SWAP]] gates. This is expensive - each [[SWAP]] is $3$ CNOTs on real hardware.
### Noise Simulation Without Real Hardware
To simulate hardware-realistic errors offline:
```python
from qiskit_aer import AerSimulator
from qiskit_ibm_runtime.fake_provider import FakeSherbrooke

noisy_sim = AerSimulator.from_backend(FakeSherbrooke())
job = noisy_sim.run(tqc, shots=1024)   # direct AerSimulator.run() - not Qiskit Runtime
counts = job.result().get_counts()      # get_counts() works here (local path, not SamplerV2)
print(counts)  # shows realistic error rates without queue wait
```
`FakeSherbrooke` ($127$ [[Qubits]]) & `FakeManilaV2` ($5$ [[Qubits]]) are good choices. This is the fastest debugging loop: simulate noise locally before spending QPU time. Note: `get_counts()` works here because this is direct `AerSimulator.run()`, not Qiskit Runtime - on real hardware use `SamplerV2` & extract via `result[0].data.meas.get_counts()`.
### Reading Real Hardware Calibration
```python
props = backend.properties()
for gate in props.gates:
    if gate.gate == 'cx':
        for param in gate.parameters:
            if param.name == 'gate_error':
                print(f"CX({gate.qubits}): error={param.value:.4f}")
```
This tells you which qubit pairs have the lowest [[CNOT]] error right now. If you have flexibility in which [[Qubits]] to use, pick the best ones. Calibration data refreshes every few hours.

Source: [Qiskit documentation](https://docs.quantum.ibm.com/)

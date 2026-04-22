#Quantum #Cloud #Platform

| Goal                                        | Best platform           | Why                                                  |
| ------------------------------------------- | ----------------------- | ---------------------------------------------------- |
| Learning quantum computing from scratch     | **IBM Quantum**         | Largest community, best tutorials, Qiskit everywhere |
| Structured algorithm exercises              | **Azure Quantum Katas** | Best curated problem set, Q# is rigorous             |
| Quantum ML / variational algorithms         | **PennyLane**           | Autodiff integration, device abstraction             |
| FTQC algorithm design & resource estimation | **Azure Quantum**       | Resource Estimator is unmatched                      |
| Combinatorial optimization                  | **D-Wave Leap**         | Only platform built for QUBO/Ising                   |
| Comparing hardware providers                | **Amazon Braket**       | IonQ, Rigetti, QuEra in one SDK                      |
| Running same circuit on many backends       | **qBraid**              | Unified transpiler across $34+$ devices              |

## IBM Quantum - Practical Guide

### Step-by-step getting started
1. Create IBM ID at `quantum.cloud.ibm.com` - no credit card needed
2. Install: `pip install qiskit qiskit-ibm-runtime qiskit-aer`
3. Save API token: `QiskitRuntimeService.save_account(channel="ibm_quantum", token="...")`
4. Write circuit → **test locally with AerSimulator first** → submit to real backend

### What actually matters

**Transpilation is not optional.** Every circuit submitted to real hardware goes through `transpile()`. Native gate Same as `auto` - synonym used i- older Q- syntax. Always check `transpile(qc, backend).depth()` before submitting.

```python
from qiskit import transpile
from qiskit_ibm_runtime import QiskitRuntimeService

service = QiskitRuntimeService()
backend = service.least_busy(operational=True, simulator=False)

# Check compiled circuit depth before spending QPU time
tqc = transpile(qc, backend, optimization_level=3)
print(f"Original depth: {qc.depth()}, Compiled: {tqc.depth()}")
```

**FakeBackends are your real dev tool.** They simulate device noise using real calibration snapshots - no queue, instant results, realistic error behavior:

```python
from qiskit_aer import AerSimulator
from qiskit_ibm_runtime.fake_provider import FakeSherbrooke

noisy_sim = AerSimulator.from_backend(FakeSherbrooke())
# test as if on real hardware, no queue time
```

**Queue times are real.** `least_busy()` helps but during peak hours even the "least busy" backend has a 30-minute queue. Submit jobs & come back. Do not sit & wait.

**Calibration data changes daily.** Gate error rates, T1/T2 times, & readout fidelity are recalibrated every ~24 hours. A qubit that was 0.1% error yesterday may be 0.3% today. Always check live calibration on the backend dashboard before choosing qubits manually.

**The SamplerV2 / EstimatorV2 split is intentional.** These are the only way to run on real hardware now - `execute()` is removed in Qiskit 1.0+. Use `SamplerV2` when you want measurement counts (most circuits), `EstimatorV2` when you want `<ψ|O|ψ>` expectation values ([[VQE]], [[QAOA]]).

### What is not intuitive
- **Heavy-hex topology**: qubits are not all-to-all connected. If your circuit applies CNOT between non-adjacent qubits, the compiler inserts SWAP gates - each SWAP costs 3 CNOTs. This can triple your circuit depth silently.
- **Readout error is separate from gate error** - & often larger. A circuit with 99% gate fidelity can give garbage results if readout error is 5%. Apply measurement error mitigation (M3 from `mthree` library).
- **More shots ≠ always better** - for sampling tasks 1024 is usually enough. For expectation values in [[VQE]] you may need 10k+ shots to reduce shot noise below the energy gap you're resolving.
- **ibmq_qasm_simulator is a noiseless cloud simulator** - results will look perfect. This is not representative of hardware. Switch to `AerSimulator.from_backend()` early.

---

## Azure Quantum & Q# - Practical Guide

### Step-by-step getting started
1. Create Azure account → create Quantum Workspace in Azure portal
2. Install: `pip install azure-quantum qsharp`
3. For Q# only (no Azure): install VS Code + QDK extension - runs locally, no account needed
4. Work through [[Quantum Katas]] first - they teach Q# idioms before you write your own circuits

### What actually matters

**The Resource Estimator is the most valuable tool here.** Run it on any non-trivial algorithm before writing a single line of QPU code. It will tell you exactly how many physical qubits, T-gates, & years of wall-clock time your algorithm needs on fault-tolerant hardware. Most textbook algorithms turn out to require millions of qubits - knowing this early saves enormous time.

```python
# Runs via Q# + Azure Resource Estimator
# shows: physical qubit count, runtime, T-factory overhead
```

**[[Quantum Katas]] are worth doing in order.** They are structured differently from most tutorials - each kata has multiple tasks that build on each other, & you must pass all assertions before proceeding. The Q# simulator is the test runner. This forces correctness, not just "it runs."

**`DumpMachine()` is your debugger.** Unlike classical debugging, you cannot inspect qubits without collapsing them on hardware. In simulation, `DumpMachine()` prints the full statevector - every amplitude & phase. Use it aggressively during dev:

```csharp
use q = Qubit();
H(q);
DumpMachine(); // prints: |0⟩ 0.707, |1⟩ 0.707
// verify state is correct before applying next gate
```

**`within/apply` prevents ancilla mistakes.** Any uncomputation pattern (apply, do work, undo) should use `within/apply`, not manual `Adjoint` calls. The compiler automatically generates the adjoint of the within block:

```csharp
within {
    H(ancilla);
    CNOT(data, ancilla);
} apply {
    Controlled op([ancilla], target); // ancilla cleaned up automatically
}
```

**[[Adaptive profile]] is required for practical FTQC.** The `Base` profile compiles for hardware with no classical feed-forward. `AdaptiveRI` enables mid-circuit measurement results to drive subsequent gates - required for teleportation, magic state injection, & [[Iterative QPE]].

### What is not intuitive
- **Q# simulator is limited to ~29-30 qubits** (statevector stores $2^n$ complex numbers in RAM). This is not a soft limit - it's RAM. At 30 qubits you need 16 GB.
- **Qubits in Q# start as $|0
angle$ & must be returned as $|0
angle$** - leaking non-zero qubits out of a `use` block is a runtime error in simulation. Uncompute ancillas with `Reset()` or `ResetAll()`.
- **`is Adj + Ctl` has to be satisfied everywhere** - if your operation calls another operation that doesn't support `Ctl`, your operation can't be marked `is Ctl`. This cascades up the call stack.
- **Q# 1.0 uses `import`, not `open`** - most online examples pre-2023 use `open Microsoft.Quantum.*`. Current syntax is `import Std.Arrays.*;`. Mixing them causes compile errors.
- **The Azure credit system has 3 currencies** ([[Azure Quantum Credits]]): AQC (hardware credits), AQT (IonQ tokens), HQT (Quantinuum tokens). These are not interchangeable. Running out of one doesn't affect the others.

---

## PennyLane - Practical Guide

### Step-by-step getting started
1. `pip install pennylane pennylane-qiskit` (or `pennylane-braket` for AWS backend)
2. Start with `default.qubit` device - noiseless local simulation
3. Learn `qml.qnode` decorator + `qml.device` before anything else
4. Add real hardware backend only after algorithm is verified on simulator

### What actually matters

**The device abstraction is the point.** The same `@qml.qnode` circuit runs on IBM hardware, AWS Braket, a GPU simulator, or the default CPU simulator - just by changing the `device` argument. This means you write once & benchmark everywhere:

```python
import pennylane as qml

dev_cpu = qml.device("default.qubit", wires=4)
dev_gpu = qml.device("lightning.gpu", wires=4)      # 100x faster for large sims
dev_ibm = qml.device("qiskit.ibmq", wires=4, backend="ibm_nairobi")

@qml.qnode(dev_cpu)   # swap to dev_gpu or dev_ibm without changing circuit
def circuit(params):
    qml.RX(params[0], wires=0)
    qml.CNOT(wires=[0, 1])
    return qml.expval(qml.PauliZ(0))
```

**Parameter-shift gradient is the core QML primitive.** Classical autodiff (backprop) cannot compute gradients through quantum hardware - it requires evaluating the circuit. The parameter-shift rule evaluates the circuit at `θ ± π/2` to compute the exact gradient. PennyLane handles this automatically with `qml.grad` or by setting `diff_method="parameter-shift"`:

```python
grad_fn = qml.grad(circuit)
gradients = grad_fn(params)  # exact gradient via parameter-shift, works on real hardware
```

**Catalyst (JIT) is worth enabling for variational loops.** If your optimization loop calls the circuit hundreds of times, `@qml.qjit` compiles the circuit + classical optimizer into a single compiled program - no Python overhead per iteration:

```python
@qml.qjit
@qml.qnode(dev_gpu)
def fast_circuit(params):
    ...
```

### What is not intuitive
- **`diff_method="backprop"` only works on simulators** - it uses the statevector directly. On real hardware or shot-based simulators you must use `"parameter-shift"` or `"finite-diff"`.
- **Shots affect gradient variance** - with `shots=1024` the parameter-shift gradient is noisy. Increase shots during optimization or use analytic mode (shots=None) on simulators.
- **Entanglement structure matters for expressibility** - not all ansatz circuits learn all functions. The `StronglyEntanglingLayers` template is a safe default; custom ansatze often underperform.
- **Strawberry Fields / photonic** is a completely separate paradigm inside PennyLane - continuous-variable, Gaussian operations, not qubit-based. Don't confuse `default.qubit` with `strawberryfields.fock`.

---

## Amazon Braket - Practical Guide

### Step-by-step getting started
1. Create AWS account → enable Amazon Braket in a supported region (us-east-1, us-west-2, eu-west-2)
2. `pip install amazon-braket-sdk`
3. Always start with `LocalSimulator` - it's free & instant, runs on your machine
4. Use `SV1`/`DM1` for cloud sim before touching any QPU

### What actually matters

**The local simulator is your primary tool:**
```python
from braket.devices import LocalSimulator
device = LocalSimulator()   # free, no AWS account charges, up to ~25 qubits
```

**Use `DM1` for realistic noise modeling** - `SV1` is noiseless. If you want to understand how your circuit will behave on real hardware, `DM1` (Density Matrix simulator) applies depolarizing noise & is more representative than `SV1`.

**Braket is the best platform for cross-provider hardware comparison.** IonQ (trapped ion) vs. Rigetti (superconducting) vs. QuEra (neutral atom analog) have fundamentally different error profiles, gate sets, & topologies. Running the same circuit on all three & comparing results teaches more than any textbook.

**QPU costs are per-task + per-shot.** A 1000-shot IonQ job costs `$0.30 + 1000 × $0.03 = $30.30`. Test extensively on simulators & keep shot counts low on hardware. Set AWS billing alerts.

### What is not intuitive
- **S3 bucket is required for QPU jobs** - cloud QPU results are stored in S3, not returned directly. You must create a bucket & pass `s3_destination_folder` to every QPU task. This is infrastructure overhead that surprises beginners.
- **QuEra Aquila is analog, not gate-based** - you program it with `AnalogHamiltonianSimulation` specifying atom positions & laser drive profiles, not quantum gates. It's a completely different programming model.
- **TN1 (Tensor Network simulator)** is only efficient for circuits with limited entanglement. Deeply entangled circuits will timeout even at low qubit counts. It shines on circuits with local interactions (e.g., nearest-neighbor CNOT chains).
- **Hybrid Jobs has cold-start overhead** - spinning up the EC2 instance + QPU access takes 2-5 minutes. Not suitable for quick experiments; valuable only for long iterative algorithms.

---

## D-Wave Leap - Practical Guide

### Step-by-step getting started
1. Create account at `cloud.dwavesys.com` - free exploration time available
2. `pip install dwave-ocean-sdk`
3. Authenticate: `dwave config create`
4. **Learn QUBO formulation first** - this is the entry barrier; everything else follows

### What actually matters

**D-Wave is a different paradigm entirely.** There are no qubits, gates, or circuits in the sense of IBM/Azure. Instead, you define an energy function over binary variables & the annealer finds the minimum energy state. The mapping from your problem to QUBO is the entire challenge.

**Hybrid solvers outperform raw QPU for real problems.** `LeapHybridSampler` combines QPU sampling with classical heuristics & handles thousands of variables. Raw QPU access is better for research/benchmarking:

```python
from dwave.system import LeapHybridSampler
sampler = LeapHybridSampler()
# handles large problems that don't fit on QPU topology
result = sampler.sample_qubo(Q, time_limit=5)
```

**Embedding is the hidden complexity.** The Pegasus topology is not all-to-all - your logical variable graph must be embedded onto physical qubits. `EmbeddingComposite` handles this automatically but may use many physical qubits for a few logical variables (chain of physical qubits = 1 logical qubit). Chain strength matters: too low → chains break → wrong answers; too high → qubits fight each other.

**Use `dimod` for problem formulation, not raw dicts.** The `BinaryQuadraticModel` class handles QUBO, Ising, & constraint models cleanly & integrates with all solvers.

### What is not intuitive
- **Not useful for gate-model algorithms** - Grover, QPE, Shor, VQE are all gate-model. D-Wave cannot run them. It is an optimizer, not a universal quantum computer.
- **Multiple reads give a distribution, not one answer** - you get 100-1000 candidate solutions; the best one is usually correct but not guaranteed. Always take `response.first.sample`.
- **Problem structure affects quality dramatically** - a well-formulated QUBO on D-Wave can beat classical solvers; a poorly formulated one gives random-looking results.
- **Advantage2 uses Pegasus topology** - different from older Chimera topology in Advantage. Embedding code written for one may not work on the other.

---

## Universal Gotchas (all platforms)

**Simulation ≠ hardware.** A circuit that gives textbook results in simulation will give noisy, error-affected results on any real QPU. The gap is much larger than beginners expect. This is not a bug - it is the current state of the field.

**Circuit depth is the enemy.** Every gate adds error. Every SWAP gate (routing) adds 3 CNOTs. Shallower circuits always outperform deeper circuits on NISQ hardware, even if they're logically equivalent. Optimize for depth, not qubit count.

**Choose qubits manually for critical circuits.** Calibration data shows which qubit pairs have the best CNOT fidelity. For 2-qubit circuits, routing to the best physical pair is worth doing manually.

**Shots, not outcomes.** Quantum measurement is probabilistic. You run the circuit `N` shots & get a distribution. Rare outcomes need exponentially more shots to resolve. `shots=1024` is a starting point, not a universal answer.

**Queue time is part of the experiment.** On IBM Quantum, submitting a job & getting results back can take hours. Design your workflow around asynchronous execution - submit a batch of experiments, come back, analyze.

---

## Recommended Learning Path

1. **Start with IBM Quantum + Qiskit** - run Bell state on `AerSimulator`, then on a real backend. Feel the difference.
2. **Do Azure Quantum Katas** - work through Superposition & Measurements kata sets. Q# forces you to think in terms of unitaries, not circuits.
3. **Run Resource Estimator on Grover's algorithm** - see that it needs millions of qubits. Grounds expectations about FTQC timelines.
4. **Try PennyLane for a VQE problem** - understand parameter-shift gradients & why optimization is hard.
5. **Try D-Wave on a real optimization problem** (TSP, graph coloring) - understand QUBO formulation.
6. **Use Amazon Braket to run the same Bell circuit on IonQ & Rigetti** - compare fidelity & understand hardware differences.

---

## Sources
- [IBM Quantum documentation](https://docs.quantum.ibm.com)
- [Qiskit Patterns & learning resources](https://learning.quantum.ibm.com)
- [Azure Quantum Resource Estimator](https://learn.microsoft.com/en-us/azure/quantum/intro-to-resource-estimation)
- [PennyLane key concepts](https://pennylane.ai/qml/glossary/)
- [PennyLane parameter-shift](https://pennylane.ai/qml/glossary/parameter_shift/)
- [Amazon Braket developer guide](https://docs.aws.amazon.com/braket/latest/developerguide/what-is-braket.html)
- [D-Wave Ocean SDK: problem formulation](https://docs.ocean.dwavesys.com/en/stable/concepts/index.html)
- [D-Wave embedding concepts](https://docs.dwavesys.com/docs/latest/c_gs_workflow.html)

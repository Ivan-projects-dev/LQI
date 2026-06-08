#SoftDev #Hardware #Algorithm 
- **[[IBM Quantum]]** - you want to run real circuits on real hardware, starting today, for free
- **[[Azure Quantum]] / Q#** - you want to learn quantum algorithms properly, or you need to know fault-tolerant resource costs before committing to hardware
- **[[PennyLane]]** - you are training quantum circuit (gradients, optimization, QML)
- **[[Amazon Braket]]** - you need specific hardware vendor ([[IonQ]], [[Rigetti]], QuEra) or AWS integration
- **[[D-Wave]]** - your problem is combinatorial optimization that fits [[QUBO]] form
### Decision by Goal
### 1. Learning quantum computing from scratch
**Best: Azure [[Quantum Katas]] + [[IBM Quantum]] Learning**

Azure Katas gives immediate math feedback - your implementation is tested against the correct statevector, not just "does it run." [[IBM Quantum]] Learning has the best-structured course sequence. Neither requires hardware access to start. Do not start with [[D-Wave]] - it uses different paradigm that will confuse your understanding of gate-based quantum computing.
### 2. Need to run circuit on real quantum hardware
**[[IBM Quantum]]** has the most accessible free QPU tier: no credit card, queue time measured in minutes (not hours), & the queue is shared with research users so off-peak access is fast. `SamplerV2` API is straightforward.

Azure requires Azure account + workspace for hardware access. Braket requires AWS account + $S3$ setup. Both have $>$ friction than IBM for $1st$ QPU run.
### 3. Need to implement & test variational algorithm (VQE, QAOA, QML)
**[[PennyLane]]** is built around differentiable circuits. Gradient infrastructure (parameter-shift, adjoint, backprop) is  mature than any other platform. Ecosystem of optimizers, QML layers, & demos is richer. Qiskit has [[VQE]]/[[QAOA]] support via `qiskit_algorithms`, but the gradient pipeline is  polished. Q# has no gradient support. 

[[D-Wave]] cannot do variational algorithms at all.
### 4. Checking if algorithm can run on FTQC in $10$ years
**[[Azure Quantum]] Resource Estimator** ([[QRE]]) is the only tool that gives concrete qubit count $+$ runtime estimate for full fault-tolerant implementation. It accounts for error correction overhead, T-factory count, code distance, & qubit model. It is free, requires no account, & runs in $5$ seconds.

[[D-Wave]] is not relevant for fault-tolerant algorithms.
### 5. Solving combinatorial optimization problem
**[[D-Wave]] (for [[QUBO]]-expressible problems) or [[QAOA]] via [[PennyLane]]/Qiskit (for gate-based)**

If your problem is known optimization problem (max-cut, job scheduling, portfolio optimization, graph coloring) & you can formulate [[QUBO]]: [[D-Wave]] is the purpose-built tool. It handles problems too large for simulators & has mature tooling (Ocean SDK, built-in [[QUBO]] formulations for common problems).

If you want gate-based approach for comparison or research: implement [[QAOA]] in [[PennyLane]] or Qiskit. [[QAOA]] on current hardware is limited to small instances ($~20$ variables) due to circuit depth.
### 6. Accessing & comparing multiple different QPUs
**[[Amazon Braket]]** is only SDK that gives unified interface to [[IonQ]] (trapped ion), [[Rigetti]] (superconducting), OQC (superconducting), & QuEra (neutral atom / Rydberg). If research involves comparing hardware tech, Braket saves you from maintaining four separate SDKs.

[[IBM Quantum]] only provides IBM hardware. Azure only provides [[IonQ]], [[Quantinuum]], & [[Rigetti]]. Braket has the broadest hardware coverage.
### 7. Trying photonic quantum computing
**[[Xanadu]] [[Strawberry Fields]] ([[PennyLane]] ecosystem)**
[[Xanadu]] builds photonic quantum computers. [[Strawberry Fields]] is the SDK for **continuous-variable quantum computing**.

| Coming from  | Moving to         | What changes                                                                                                 |
| ------------ | ----------------- | ------------------------------------------------------------------------------------------------------------ |
| IBM / Qiskit | Q# / Azure        | No circuit object - operations are funcs. DumpMachine replaces statevector inspection. Reset is mandatory.   |
| IBM / Qiskit | [[PennyLane]]         | Circuit is a Python func with `@qml.qnode`. Return type determines output. `requires_grad=True` is required. |
| IBM / Qiskit | [[D-Wave]]            | No gates, no circuits, no measurement operators. [[QUBO]] formulation replaces everything.                       |
| Q# / Azure   | IBM / Qiskit      | `measure_all()` is manual. Transpile is mandatory. Result extraction from `SamplerV2` is verbose.            |
| [[PennyLane]]    | [[D-Wave]]            | Gradient-based training does not apply. Annealing is probabilistic - no gradient to follow.                  |
| [[D-Wave]]       | Any gate platform | Start from the basics. [[D-Wave]] experience does not transfer to gate-based computing.                          |
### Hardware Connectivity for algorithm
Non-obvious factor: the qubit **connectivity graph** of the hardware determines how many [[SWAP]] gates get inserted during transpilation.

| Platform             | Connectivity       | Impact                                                       |
| -------------------- | ------------------ | ------------------------------------------------------------ |
| IBM (Eagle/Heron)    | Heavy-hex topology | Limited - non-adjacent [[Qubits]] need [[SWAP]] chains               |
| [[Quantinuum]] $H_1/H_2$ | All-to-all         | No [[SWAP]] overhead - any $2$ [[Qubits]] can interact directly      |
| [[IonQ]] Aria            | All-to-all         | Same - ideal for algorithms with many non-local interactions |
| [[Rigetti]]              | Limited grid       | [[SWAP]] overhead similar to IBM                                 |
| [[D-Wave]] Pegasus       | Fixed sparse graph | Embedding maps logical variables to physical qubit chains    |
**Practical impact:** circuit with many $2$-qubit gates between non-adjacent [[Qubits]] (e.g., a fully connected [[QAOA]] problem) will have $3-5x$ $>$ gates after transpilation on IBM than on [[Quantinuum]], because each needed [[CNOT]] between non-adjacent [[Qubits]] requires $2-3$ [[SWAP]] gates, each costing $3$ CNOTs. If your circuit has high connectivity demands, [[Quantinuum]] or [[IonQ]] (accessible via Azure or Braket) produces better results despite slower clock speeds.

| Platform                   | Error type                         | Typical [[CNOT]] error              |
| -------------------------- | ---------------------------------- | ------------------------------- |
| IBM Superconducting        | Decoherence, gate error, crosstalk | $0.3\%$-$1\%$                   |
| [[Quantinuum]] Trapped Ion | Phonon modes, slower gates         | $0.1\%$-$0.5\%$                 |
| [[IonQ]] Trapped Ion       | Similar to [[Quantinuum]]          | $0.2\%$-$0.8\%$                 |
| [[D-Wave]] Annealer        | Chain breaks, thermal noise        | Not applicable (not gate-based) |
Trapped-ion hardware has $<$ error rates per gate but is $1000x$ slower clock speed than superconducting. For shallow circuit ($< 50$ gates), trapped-ion gives better fidelity. For deep circuit that would take hours on trapped-ion, superconducting finishes in seconds at the cost of higher per-gate error.


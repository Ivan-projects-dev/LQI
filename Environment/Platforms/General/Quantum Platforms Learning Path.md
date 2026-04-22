#Quantum #Beginner
Recommended order for getting started, based on what builds understanding most efficiently.

## Step 0: $3$ First Experiences

Before anything else, do these $3$ things in order:

1. Go to `learning.quantum.ibm.com`. Complete the **"Basics of Quantum Information"** course. It runs entirely in the browser with interactive Jupyter notebooks and the Qiskit simulator. No account setup, no hardware credits spent.

2. Install Q# (VS Code + QDK extension) and complete the first $2$ [[Quantum Katas]] sets: **Superposition** and **Measurements**. These force you to implement circuits that pass automated correctness checks. You cannot proceed with a wrong answer - the test fails and tells you what the [[Quantum state]] should have been.

3. Run a Bell state on a **real IBM QPU once**. Compare the histogram to the same circuit on `AerSimulator`. The gap between perfect simulation results and noisy hardware results is the most important intuition in quantum computing. Seeing it firsthand is worth more than a chapter of theory.

## Full Learning Sequence

**Phase 1 - Foundations (gate-model)**
Start with [[IBM Quantum]] + Qiskit. Largest community, most Stack Overflow answers, most tutorials. Write [[Bell states]], GHZ states, simple oracles. Use `AerSimulator` for development, `FakeBackends` for noise simulation, real QPU sparingly.

**Phase 2 - Rigorous language & algorithm theory**
Move to [[Azure Quantum]] Q# and [[Quantum Katas]]. Q# forces you to think in terms of unitaries and adjoints, not just gate sequences. Complete Superposition, Measurements, [[Multi-qubit gates]], and Oracles kata sets.

**Phase 3 - Reality check**
Run the [[Quantum resource estimator]] on [[Grover]]'s algorithm and Shor's algorithm. See how many physical [[Qubits]] and years of runtime each requires. This grounds expectations about what is runnable today vs. what is future [[FTQC]] territory.

**Phase 4 - Variational algorithms & QML**
Try [[Xanadu PennyLane]] for a [[VQE]] or [[QAOA]] problem. Understand how parameter-shift gradients work. Run the same circuit on `default.qubit` then on a real backend via `qiskit.ibmq`. See how shot noise affects gradient estimates.

**Phase 5 - Optimization problems**
Try [[D-Wave Leap]]. Formulate a small TSP or graph coloring problem as [[QUBO]]. Submit to the annealer. Understand the completely different paradigm - energy minimization, not circuit execution.

**Phase 6 - Hardware comparison**
Use [[Amazon Braket]] to run the same Bell state on IonQ (trapped ion) and Rigetti (superconducting). Compare the measurement distributions and gate fidelity. Understand why different qubit technologies have different error profiles.

## Platform-Purpose Quick Reference

| Goal | Platform |
|---|---|
| Learn quantum basics | [[IBM Quantum]] + Qiskit |
| Rigorous structured exercises | [[Azure Quantum]] Katas |
| [[FTQC]] algorithm design | [[Azure Quantum]] Resource Estimator |
| Quantum ML / variational | [[Xanadu PennyLane]] |
| Combinatorial optimization | [[D-Wave Leap]] |
| Multi-hardware comparison | [[Amazon Braket]] |
| Run same circuit on 34+ backends | [[qBraid]] |

## Sources
- [IBM Quantum Learning](https://learning.quantum.ibm.com)
- [Qiskit Getting Started](https://docs.quantum.ibm.com/start)
- [Azure Quantum Learning path](https://learn.microsoft.com/en-us/training/paths/quantum-computing-fundamentals/)
- [PennyLane tutorials](https://pennylane.ai/qml/demonstrations/)
- [D-Wave Ocean SDK intro](https://docs.ocean.dwavesys.com/en/stable/getting_started.html)
- [Amazon Braket getting started](https://docs.aws.amazon.com/braket/latest/developerguide/braket-get-started.html)

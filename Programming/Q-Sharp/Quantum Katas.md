#Quantum #Q-Sharp #Learning
**Quantum Katas** are Microsoft's open-source, self-paced interactive tutorials for learning quantum computing & Q# programming. Available at [quantum.microsoft.com/tools/quantum-katas](https://quantum.microsoft.com/en-us/tools/quantum-katas) & on [GitHub (microsoft/QuantumKatas)](https://github.com/microsoft/QuantumKatas).

## Structure

Each kata is a self-contained module covering one topic. Tasks progress from simple to challenging — each task presents a quantum operation with a missing implementation, & the testing framework validates your solution automatically.

Katas run directly in the browser via **Copilot in Microsoft Quantum** (no local setup) or locally as Q# projects using the QDK.

## Topics Covered

**Quantum basics**
- [[Single-qubit gates]]: X, H, S, T, Rz & their [[Matrix]] representations
- [[Multi-qubit gates]]: [[CNOT]], Toffoli, SWAP
- [[Bell states]] & entanglement
- Superposition & interference

**Measurement**
- Measurement in the $Z, X$, & Bell bases
- Distinguishing quantum states

**Algorithms**
- [[Algorithms/Deutsch-Jozsa/Deutsch-Jozsa|Deutsch-Jozsa]]
- [[Algorithms/Bernstein-Vazirani/Bernstein-Vazirani|Bernstein-Vazirani]]
- [[Algorithms/Simon/Simon|Simon's algorithm]]
- [[Algorithms/Grover/Grover|Grover's search]]
- [[QPE|Quantum Phase Estimation]]

**Q# lang**
- Operations vs functions
- [[Functors]] (Adjoint, Controlled)
- [[Qubits/Ancilla|Ancilla]] [[Qubit management]]
- Classical control flow in Q#

**Error correction** (intro)
- [[FTQC/Bit-flip code|Bit-flip code]]
- Phase-flip code
- Shor 9-qubit code

## Running Locally

```bash
git clone https://github.com/microsoft/QuantumKatas.git
cd QuantumKatas/<kata-name>
dotnet run
```

Requires .NET SDK & the QDK VS Code extension.

## Copilot Integration

Within quantum.microsoft.com you can ask Copilot to:
- Explain the concept behind any kata task
- Give progressive hints without revealing the full solution
- Debug & explain Q# compilation errors in context

## Sources
- [Quantum Katas (quantum.microsoft.com)](https://quantum.microsoft.com/en-us/tools/quantum-katas)
- [GitHub: microsoft/QuantumKatas](https://github.com/microsoft/QuantumKatas)
- [Quantum learning resources (Azure)](https://azure.microsoft.com/en-us/resources/training-and-certifications/quantum-computing)

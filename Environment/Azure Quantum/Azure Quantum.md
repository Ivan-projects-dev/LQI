#Quantum #Cloud #Q-Sharp 
When you compile & run quantum program, the QDK creates instance of the quantum simulator & passes the Q# code to it. Simulator uses the Q# code to create [[Qubits]] (simulations of quantum particles) & apply transformations to modify their state. Results of the quantum operations in the simulator are then returned to the program. Isolating the Q# code in the simulator ensures that the algorithms can run correctly on quantum computers.

When you run quantum program in Azure Quantum, you create & run **job**. To submit job to the [[Azure Quantum Providers]], you need Azure account & quantum workspace. Once you submit your job, Azure Quantum manages the job lifecycle, including job scheduling, exec, & monitoring. You can track the status of your job & view the results in the Azure Quantum portal.
## Key Components

**Quantum Development Kit (QDK)** - open-source SDK supporting Q#, Qiskit, Cirq, & [[OpenQASM]]. Provides language extensions for VS Code, Jupyter notebook integration, domain libraries for chemistry & ML, & built-in sparse-state simulator. QDK compiles programs to **QIR (Quantum Intermediate Representation)** - common LLVM-based IR that lets any supported lang target any provider backend.

**Azure Quantum Resource Estimator ([[QRE]])** - estimates num of physical & logical [[Qubits]], runtime, & T-factory counts required to run an algorithm at fault-tolerant scale. Supports configurable qubit parameter sets (superconducting, trapped-ion), two built-in QEC schemes ([[Surface Code]], floquet code), & custom error correction models. Computes space-time tradeoff frontiers to balance qubit count vs. runtime. Completely **free** - no Azure account required; runs in VS Code or at quantum.microsoft.com.

**Copilot in Microsoft Quantum** - AI assistant (GPT-4 powered) at quantum.microsoft.com. Lets you write, explain, & execute Q# directly in the browser against an in-memory simulator or the free Quantinuum emulator. Useful for learning via Quantum Katas & exploring sample code without setting up a local environment.

**Simulators** - beyond provider-offered simulators, the QDK ships a local **sparse-state simulator** (handles large qubit counts efficiently when state is sparse) & a **noise model simulator** for prototyping error-aware circuits. The free **Quantinuum H1 emulator** is accessible via quantum.microsoft.com without an Azure subscription.

**Target profiles** define what operations a target supports:
- `Base` - basic quantum gates, no classical control within a shot
- `Adaptive RI` - supports mid-circuit measurement, classical conditionals, & qubit reuse within a single shot (required for integrated hybrid)
- `Unrestricted` - used by local simulators (no hardware constraints)

## Sources
- [What is Azure Quantum?](https://learn.microsoft.com/en-us/azure/quantum/overview-azure-quantum)
- [QDK overview](https://learn.microsoft.com/en-us/azure/quantum/qdk-main-overview)
- [Azure Quantum Resource Estimator](https://learn.microsoft.com/en-us/azure/quantum/intro-to-resource-estimation)
- [Copilot in Microsoft Quantum](https://learn.microsoft.com/en-us/azure/quantum/get-started-azure-quantum)
- [Backend simulators](https://learn.microsoft.com/en-us/azure/quantum/backend-simulators)
- [Quantum Roadmap](https://quantum.microsoft.com/en-us/vision/quantum-roadmap)

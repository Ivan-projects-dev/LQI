#SoftDev #Q-Sharp #ML
When you compile & run quantum program, the QDK creates instance of the quantum simulator & passes the Q# code to it. Simulator uses the Q# code to create [[Qubits]] (simulations of quantum particles) & apply transformations to modify their state. Results of the quantum operations in the simulator are then returned to the program. Isolating the Q# code in the simulator ensures that the algorithms can run correctly on quantum computers.

When you run quantum program in Azure Quantum, you create & run **job**. To submit job to the [[Azure Providers]], you need Azure account & quantum workspace. Once you submit your job, Azure Quantum manages the job lifecycle, including job scheduling, exec, & monitoring. You can track the status of your job & view the results in the Azure Quantum portal.

**Quantum dev Kit (QDK)** - open-source SDK supporting Q#, Qiskit, Cirq, & [[OpenQASM]]. Provides language extensions for VS Code, Jupiter notebook integration, domain libraries for chemistry & ML, & built-in sparse-state simulator. QDK compiles programs to **QIR (Quantum Intermediate Representation)** - common LLVM-based IR that lets any supported lang target any provider backend.

**Azure [[QRE]]** - estimates num of physical & logical [[Qubits]], runtime, & T-factory counts required to run an algorithm at fault-tolerant scale. Supports configurable qubit parameter sets (superconducting, trapped-ion), $2$ built-in QEC schemes ([[Surface Code]], floquet code), & custom error correction models. Computes space-time tradeoff frontiers to balance qubit count vs. runtime. Completely **free** - no Azure account required; runs in VS Code or at quantum.microsoft.com.

**Copilot in Microsoft Quantum** - AI assistant ($GPT-4$ powered) at quantum.microsoft.com. Lets you write, explain, & execute Q# directly in the browser against an in-memory simulator or the free [[Quantinuum]] emulator. Useful for learning via [[Quantum Katas]] & exploring sample code without setting up a local env.

**Simulators** - beyond provider-offered simulators, the QDK ships local **sparse-state simulator** (handles large qubit counts efficiently when state is sparse) & a **noise model simulator** for prototyping error-aware circuits. Free **[[Quantinuum]] H1 emulator** is accessible via quantum.microsoft.com without Azure subscription.

**Target profiles** define what operations target supports:
- `Base` - basic quantum gates, no classical control within shot
- `Adaptive RI` - supports mid-circuit measurement, classical conditionals, & qubit reuse within a single shot (required for integrated hybrid)
- `Unrestricted` - used by local simulators (no hardware constraints)

**Run the Resource Estimator before writing hardware code.** [[QRE]] accepts any Q# operation & returns physical qubit count, T-factory overhead, & wall-clock runtime on [[FTQC]] hardware. Most textbook algorithms ([[Shor]], [[Grover]] at useful scale) require millions of physical [[Qubits]] & years of runtime. Knowing this before spending credits saves significant frustration.

**[[Quantum Katas]] are the best-structured learning resource on any platform.** Each kata is a series of tasks with auto-checked assertions. Simulator is the test runner - task only passes when your implementation is mathematically correct, not just "runs without error."

**DumpMachine() replaces print debugging in Q#.** Unlike classical debuggers, you cannot read [[Quantum state]] without collapsing it. During simulation, `DumpMachine()` prints the complete statevector: every basis state, amplitude, probability, & phase. Insert it between every gate pair when debugging new circuits.

**The simulator qubit limit is hard wall.** At $30$ [[Qubits]], the full statevector requires $16$ GB of RAM. At $33$ [[Qubits]] it requires $128$ GB. There is no workaround - use [[Tensor]]-network or stabilizer simulators for larger circuits.

**[[Adaptive profile]] unlocks mid-circuit measurement.** If your algorithm requires conditioning future gates on measurement outcomes (teleportation, iterative [[QPE]], magic state distillation), you must annotate the operation with `@Config(AdaptiveRI)`. Code that works on `Unrestricted` (simulation) will fail to compile for `Base` hardware targets without this.

**[[Azure Credits]] have 3 independent currencies.** AQC ([[IonQ]]/[[Rigetti]]), AQT ([[IonQ]] native), HQT ([[Quantinuum]]) do not convert between each other. Exhausting HQT does not affect AQC balance.

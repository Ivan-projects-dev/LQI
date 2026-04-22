#Quantum #Beginner #Q-Sharp
Concrete walkthrough of starting with [[Azure Quantum]] and Q# without prior experience.

## $2$ Ways to Start (Pick the Easier One First)

**Option A - Offline, no account needed:**
Install VS Code + the **QDK extension** (search "Quantum Development Kit" in VS Code extensions). Create a file ending in `.qs`. The extension gives you a full quantum simulator, syntax highlighting, and the ability to run Q# operations locally with zero setup.

**Option B - Azure cloud:**
Create an Azure account → search "[[Azure Quantum]]" in the portal → create a Quantum Workspace. This is required for the [[Quantum resource estimator]] and real hardware access. Not needed for learning Q# itself.

**Start with Option A.**

## What Q# Looks Like

Q# is a purpose-built quantum programming language. It is not Python. It resembles a mix of C# and F#.

```csharp
import Std.Measurement.*;

operation BellState() : (Result, Result) {
    use (q0, q1) = (Qubit(), Qubit());  // allocate 2 qubits, both start |0⟩
    H(q0);                               // Hadamard: put q0 in superposition
    CNOT(q0, q1);                        // entangle q0 and q1
    return (MResetZ(q0), MResetZ(q1));   // measure and reset (required)
}
```

Key rules:
- `use` allocates [[Qubits]] - they always start as $|0
angle$
- You **must** return [[Qubits]] to $|0
angle$ before the `use` block ends. `MResetZ` measures and resets in $1$ step.
- Built-in gates: `H`, `X`, `Y`, `Z`, [[CNOT]], `T`, `S`, `Rx`, `Ry`, `Rz` and more from `Std.Intrinsic`

## Running and Debugging with DumpMachine

In simulation you can inspect the full [[Quantum state]] at any point using `DumpMachine()`:

```csharp
import Std.Diagnostics.*;

operation Debug() : Unit {
    use q = Qubit();
    H(q);
    DumpMachine(); // prints: |0⟩ amplitude=0.707, prob=0.5 | |1⟩ amplitude=0.707, prob=0.5
    let r = MResetZ(q);
    Message($"Measured: {r}");
}
```

`DumpMachine()` is like a quantum watch window. It only works in simulation - on real hardware, reading state collapses it. Use it constantly when writing new circuits.

## Quantum Katas: The Best Way to Learn

[[Quantum Katas]] are structured exercises built into the Q# environment. Each kata gives you a skeleton operation and automated tests. Your implementation must pass all assertions to proceed.

Starting katas (do in this order):
1. **Superposition** - learn how to create specific superposition states
2. **Measurements** - learn how to distinguish quantum states
3. **[[Multi-qubit gates]]** - entanglement and multi-qubit operations
4. **Oracles** - learn to encode classical functions as quantum circuits

The katas run entirely in VS Code with the local simulator. No Azure account, no QPU time.

## The Resource Estimator: Running It Once

After writing any non-trivial algorithm, run the [[Quantum resource estimator]] on it. It tells you how many physical [[Qubits]] and how much time the algorithm requires on fault-tolerant hardware:

1. Open Azure portal → your Quantum Workspace → Resource Estimator
2. Paste or upload a Q# operation
3. Choose a qubit model (e.g., `qubit_gate_us_e3`)
4. Get back: physical qubit count, T-factory count, algorithmic runtime

Expected result: most textbook algorithms need millions of physical [[Qubits]]. This is not a failure - it is the reality of the NISQ/[[FTQC]] gap.

## Sources
- [QDK Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=quantum.qsharp-lang-vscode)
- [Q# getting started](https://learn.microsoft.com/en-us/azure/quantum/install-overview-qdk)
- [Quantum Katas on GitHub](https://github.com/microsoft/QuantumKatas)
- [Azure Quantum Learning path](https://learn.microsoft.com/en-us/training/paths/quantum-computing-fundamentals/)
- [Resource Estimator quickstart](https://learn.microsoft.com/en-us/azure/quantum/quickstart-microsoft-resources-estimator)

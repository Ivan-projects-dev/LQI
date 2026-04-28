#SoftDev 
###  `Unresolved symbol: Std.Arrays`
QDK project file is missing or the import is wrong. $2$ fixes:
1. **Add `qsharp.json` project file** in the same folder as your `.qs` file:
```json
{
  "author": "you",
  "license": "MIT"
}
```
QDK extension needs this file to recognize a Q# project.
2. **Check import syntax.** Q# 1.0 uses `import`, not `open`:
```csharp
// WRONG (pre-1.0 syntax)
open Microsoft.Quantum.Arrays;

// CORRECT (Q# 1.0)
import Std.Arrays.*;
```
###  `Runtime error: Released qubits are not in the ground state`
You measured qubit but didn't reset it before the `use` block ended. Always reset:
```csharp
use q = Qubit();
H(q);
let r = M(q); // measures, but q may now be |1⟩
// WRONG: block ends here with q in unknown state

// CORRECT: use MResetZ which measures AND resets
let r = MResetZ(q);
// OR: explicitly reset
let r = M(q);
Reset(q);
```
###  `Error: Operation 'MyOp' does not support the Adjoint functor`
The operation inside your `within` block or called with `Adjoint` doesn't have `is Adj` in its signature. Every operation used in `within/apply` must support adjoint:
```csharp
// WRONG: missing is Adj
operation MyOp(q : Qubit) : Unit {
    H(q);
}

// CORRECT
operation MyOp(q : Qubit) : Unit is Adj + Ctl {
    H(q);
    // adjoint auto; is inferred for simple cases
}
```
If your operation contains measurement (`M()`), it cannot be `Adj` - measurements are irreversible.
###    `DumpMachine()` output doesn't appear
Output goes to the **Debug Console** in VS Code, not the Terminal. Open it with `Ctrl+Shift+Y` (Windows/Linux) or `Cmd+Shift+Y` (Mac). `Message()` output also goes there.
###  QDK language server takes 60+ seconds to start
Normal on first launch. The extension builds the Q# compiler in the background. Wait for the status bar at the bottom of VS Code to stop showing "Q# Loading". Subsequent launches are faster.
###  `@Config(AdaptiveRI)` operation fails to compile for Base profile
You're trying to run adaptive operation on target that only supports `Base`. Either:
- Switch to `Unrestricted` profile for simulation
- Provide `@Config(Base)` variant of the same operation (see [[Adaptive profile]])

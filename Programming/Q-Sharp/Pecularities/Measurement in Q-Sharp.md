#Q-Sharp #Quantum
**Measurement** in Q# collapses qubit's superposition to classical `Result` value (`Zero` or `One`). After measurement, the qubit is in the corresponding eigenstate. All measurement operations are in `Std.Intrinsic` or `Std.Measurement`.

**`M(q)` - Z-basis measurement**, measures qubit `q` in the Pauli-Z basis. Returns `Result`. Qubit collapses to $|0\rangle$ (`Zero`) or $|1\rangle$ (`One`).
```csharp
let result = M(q);
if result == One {
    Message("Measured |1⟩");
}
```
Equivalent to `Measure([PauliZ], [q])`. Does not reset the qubit - it remains in the post-measurement state.

**`Measure(bases, qs)` - multi-qubit Pauli measurement**, measures array of [[Qubits]] jointly in specified Pauli bases. Returns a single `Result` representing the parity of all individual outcomes.
```csharp
let r = Measure([PauliZ, PauliX], [q0, q1]); // measures Z⊗X parity
```
Useful for stabilizer measurements & error syndrome extraction in [[FTQC]].

**`MResetZ(q)` - measure & reset**, measures in $Z$-basis & immediately resets the qubit to $|0\rangle$. Most concise way to end computation:
```csharp
let result = MResetZ(q); // measure + reset in one call
```
From `Std.Measurement`. Equivalent to `M(q)` followed by `Reset(q)`.

**`MResetX(q)` & `MResetY(q)`**. Measure in $X$ or $Y$ basis & reset. Equivalent to applying the appropriate basis-change gate before measuring:
- `MResetX(q)` = `H(q); MResetZ(q)`
- `MResetY(q)` = `Adjoint S(q); H(q); MResetZ(q)`
```csharp
let xResult = MResetX(q);    // measures in |+⟩/|−⟩ basis
```

**`MeasureEachZ(qs)` - measure array in Z-basis**, measures every qubit in array; returns `Result[]`. Does not reset [[Qubits]].
```csharp
let results = MeasureEachZ(register);
```

**`MResetEachZ(qs)` - measure & reset array**, measures every qubit in $Z$-basis & resets all. Most common way to read out a multi-qubit register at the end of algorithm:
```csharp
let results = MResetEachZ(register);    // returns Result[]
```
Used in [[Grover in Q-Sharp]] & [[QPE]] in Q-Sharp for final readout.

**`ResultArrayAsIntBE(results)` - decode measurement**, converts a `Result[]` to `Int` using big-endian bit ordering. From `Std.Convert`.
```csharp
import Std.Convert.*;
let bits = MResetEachZ(register);
let value = ResultArrayAsIntBE(bits);   // e.g. [One, Zero, One] → 5
```

**Non-demolition measurement & `reset`**

`Reset(q)` measures & conditionally flips to ensure $|0\rangle$, without returning a `Result`. Use when qubit state is no longer needed but the qubit must be released cleanly:
```csharp
Reset(q);
ResetAll(qs); // reset entire array
```
Q# **requires** all [[Qubits]] to be in $|0\rangle$ before scope exit - failing to reset causes a runtime error.

Operations that contain measurements **cannot be adjoint-able** (`is Adj`). This means measurement-containing operations cannot be used inside `within { }` blocks or uncomputed. Design circuits so measurements appear only at the terminal stage, never inside [[Oracle]] bodies. See [[Ancilla]] for why this matters.
## Sources
- [Quantum memory management & qubit lifecycle](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/statements/quantummemorymanagement)
- [Std.Measurement API reference](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.measurement)
- [Std.Intrinsic: M, Measure, MResetZ](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.intrinsic)

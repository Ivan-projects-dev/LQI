#Q-Sharp
`Std.Diagnostics` provides simulation-only tools for inspecting quantum state & asserting correctness. These operations are **no-ops on real hardware** - they exist solely for dev & debugging in the Q# simulator.

**`DumpMachine()`** outputs the complete state vector of the simulator - all basis states with non-$0$ amplitude, their probability, & phase:
```csharp
use q = Qubit();
H(q);
DumpMachine();
// Output:
// |0⟩: amplitude = 0.707..., probability = 0.5, phase = 0°
// |1⟩: amplitude = 0.707..., probability = 0.5, phase = 0°
Reset(q);
```
Use after initialization, after oracle application, or between Grover iterations to verify amplitudes are evolving correctly.

**`DumpRegister(qs)`** prints state vector of specific qubit array, tracing out the rest of the system. Only meaningful when the register is not entangled with other qubits:
```csharp
use (data, anc) = (Qubit[3], Qubit());
ApplyToEach(H, data);
DumpRegister(data);    // shows state of data register only
Reset(anc);
ResetAll(data);
```

**`CheckZero(q)` - assert qubit is |0⟩**. Returns `true` if the qubit is in state $|0\rangle$. Does not collapse state:
```csharp
if not CheckZero(ancilla) {
    fail "Ancilla not properly uncomputed";
}
```
Used to verify uncomputation inside oracle bodies & `within/apply` blocks.

**`Fact(cond, msg)` - assertion**. Equivalent to classical assert - throws if `cond` is `false`:
```csharp
Fact(Length(register) > 0, "Register must be non-empty");
Fact(n % 2 == 0, "n must be even for this circuit");
```
Runs in both simulation & compilation. Does not affect quantum state.

**`CheckOperationsAreEqual(nQubits, op1, op2)`** Checks if $2$ operations produce identical unitary matrices on `nQubits` qubits. Returns `Bool`:
```csharp
let same = CheckOperationsAreEqual(1, H, q => { X(q); Z(q); X(q); });
// checks if H == XZX (they are not equal, returns false)
```
Useful for verifying that manually decomposed gate circuit matches the target unitary.

**Simulator configuration**
Q# uses a full state-vector simulator by default when run locally. The simulator stores the complete $2^n$-dimensional complex vector - feasible up to ~30 qubits on a standard machine.

For resource estimation without executing quantum operations, use the [[Quantum resource estimator]] via `Std.ResourceEstimation` - this counts T-gates, CNOTs, & qubits without simulating the state.


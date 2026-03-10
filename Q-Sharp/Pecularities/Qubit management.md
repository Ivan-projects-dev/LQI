#Q-Sharp #Quantum
**Qubit management** covers allocation, borrowing, reset, & release patterns in Q#. [[Qubits]] are a physical resource - Q# enforces strict lifecycle rules.

**`use` - clean qubit allocation**. Allocates [[Qubits]] guaranteed to be in $|0\rangle$ at the start of the scope:
```csharp
use q = Qubit(); // single qubit
use qs = Qubit[n]; // array of n qubits
use (q, qs) = (Qubit(), Qubit[3]); // tuple allocation
```

[[Qubits]] are **automatically released** at the end of the `use` block. Q# runtime checks that all [[Qubits]] are in $|0\rangle$ on release - any qubit left in another state causes runtime error.
```csharp
use q = Qubit() {
    H(q);
    // must reset q before block ends
    Reset(q);       // or MResetZ(q)
}
// q released here
```

**`borrow`** allocates [[Qubits]] in **unknown (dirty) state**. Cheaper in hardware - reuses [[Qubits]] from elsewhere in the program without reinitializing them. Program must return borrowed [[Qubits]] to their original state before the borrow block ends.
```csharp
borrow scratch = Qubit();
// use scratch as temporary space
// ... must uncompute any changes to scratch
// scratch returned to its prior state
```
Useful for implementing multi-controlled gates with fewer total [[Qubits]] - the Toffoli decomposition can use dirty [[Ancilla]] to achieve $O(n)$ circuit depth.

`Reset(q)` - measures `q` & applies `X` if result is `One`, ensuring $|0\rangle$. Returns no value.
`ResetAll(qs)` - resets every qubit in array.
```csharp
Reset(q);
ResetAll(register);
```
Distinct from `MResetZ` - `Reset` is used purely to clean up; it does not return the measurement result.

Q# supports rich array operations on qubit registers:
```csharp
let n = Length(qs); // num of qubits
let front = qs[0..2]; // qubits 0, 1, 2
let tail = qs[1...]; // all except first
let rev = qs[...-1...0]; // reversed array
let head = qs[0]; // single qubit
```
Slices return views - not copies. Passing slice to operation passes those specific [[Qubits]].

Allocate [[Qubits]] in the narrowest scope possible - this keeps the logical qubit count low & makes circuit structure clear. Compiler can reuse released [[Qubits]] for subsequent allocations.
```csharp
// Good: ancilla scoped to where it is needed
operation PhaseOracle(register : Qubit[]) : Unit is Adj {
    use anc = Qubit();
    within { X(anc); H(anc); }
    apply { markingOracle(register, anc); }
}

// Bad: allocating at top level & carrying through entire program
use anc = Qubit();
// ... many operations later
Reset(anc);
```

**`ApplyToEach` helpers**. Rather than explicit `for` loops over qubit arrays, use library combinators:
```csharp
ApplyToEach(H, qs); // H to every qubit
ApplyToEachA(X, qs); // X to every qubit (adjointable)
ApplyToEachCA(H, qs); // H (adj + ctl version)
```
`ApplyToEachA` is required inside `within { }` blocks because the auto-generated adjoint needs each individual operation to support `Adj`.

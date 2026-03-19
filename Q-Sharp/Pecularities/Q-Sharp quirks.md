#Q-Sharp
**[[Qubits]] cannot be copied or compared**

No-cloning theorem is enforced at the type level. There is no `=` copy for `Qubit`, no equality check `q1 == q2`, no storing a qubit in a classical variable:
```csharp
let copy = q; // compile error - Qubit is not copyable
if q1 == q2 { ... } // compile error - Qubit has no equality
let arr = [q]; // ok - arrays of Qubit are allowed (by reference)
```
[[Qubits]] are always passed by reference. Any operation that receives a `Qubit` is working on the same physical qubit, not a copy.

**All [[Qubits]] must be `|0⟩` on release**

Q# runtime checks every qubit's state at the end of its `use` block. Leaving a qubit in any state other than $|0\rangle$ throws runtime error:
```csharp
use q = Qubit();
H(q); // scope ends - runtime error: qubit not in |0⟩
```
Always `Reset(q)` or `MResetZ(q)` before scope exit. This enforces correct uncomputation but also means you cannot return a qubit from operation - the qubit must stay alive in the caller's scope.

**`Result`-dependent branching is restricted on hardware**

Branching on measurement result (`if M(q) == One`) is a **mid-circuit measurement**. On simulators this always works. On real hardware, classical feed-forward (using a measurement result to decide the next gate) requires hardware support - not all QPUs support it. Code that relies on it may silently fail or require specific hardware target:
```csharp
let r = M(q);
if r == One { X(q); } // ok in simulation; hardware-dependent in production
```
Operations declared `is Adj` cannot contain `Result`-conditioned branches at all - the compiler rejects them because adjoints of measurements are undefined.

**`Adjoint` cannot be generated for operations with measurements**

Any operation containing `M`, `Measure`, `MResetZ`, etc., cannot declare `is Adj`. Attempting to call `Adjoint` on such operation is a compile error. This is why [[Oracle]] bodies must be purely unitary - measurements are only at the terminal readout stage:
```csharp
operation BadOracle(q : Qubit) : Unit is Adj {
    let r = M(q); // compile error: cannot auto-generate Adjoint
    if r == One { X(q); }
}
```

**`borrow` gives dirty (unknown) [[Qubits]]**

Unlike `use`, `borrow` does not guarantee $|0\rangle$. The borrowed qubit may be in any state, entangled with something else. The circuit must work correctly for any borrowed state & must restore the qubit to its original state before the borrow block ends:
```csharp
borrow scratch = Qubit();
// scratch state is unknown - must uncompute precisely
CNOT(ctrl, scratch);
// ... use scratch ...
CNOT(ctrl, scratch); // restore scratch to original
// scratch returned
```
Incorrect uncomputation of a borrowed qubit introduces silent errors that are hard to detect.

**`%` can return negative values**

Unlike Python, Q#'s `%` operator returns a result with the same sign as the dividend. For index arithmetic that must wrap non-negatively, use `ModulusI`:
```csharp
-3 % 5 // returns -3 in Q#, not 2
ModulusI(-3, 5) // returns 2 - always non-negative
```

**Integer left-shift `<<<` not `<<`**

Q# uses triple-chevron for bit shifts to avoid ambiguity with generic type brackets:
```csharp
let n = 1 <<< 4; // 16 (not << like C# or Python)
let m = 8 >>> 1; // 4 (right shift)
```
Common mistake when porting Grover or [[QPE]] formulas from other langs.

**String interpolation requires `$"..."`**

Q# uses C#-style interpolated strings. Expressions inside `{}` are evaluated & converted to string:
```csharp
let n = 5;
Message($"n = {n}, n^2 = {n * n}");
```
`Message` is the only string output func - there is no `print` or `Console.WriteLine`.

**Operations are not funcs - they have side effects**

Calling operation twice on the same qubit applies the gate twice. Unlike func call that can be memorized, every operation call has real effect on [[Quantum state]]:
```csharp
H(q); H(q); // applies H twice - back to |0⟩, not equivalent to calling once
```
This is why `H` is its own inverse (`H² = I`) & must be explicitly applied twice to undo itself.

**`within/apply` requires `Adj` on inner operations**

Every operation called inside a `within { }` block must support `Adjoint` (declare `is Adj`). If any inner operation lacks `Adj`, the compiler cannot auto-generate the uncomputation:
```csharp
within {
    SomeOp(q); // compile error if SomeOp does not declare is Adj
} apply {
    X(q);
}
```
Always annotate [[Oracle]] helper operations with `is Adj + Ctl` even if you don't explicitly call their adjoint - `within/apply` will.

**Array slices are views, not copies**

Slicing qubit array does not allocate new [[Qubits]] - it creates  view over the same physical [[Qubits]]:
```csharp
let tail = register[1...];
H(tail[0]); // modifies register[1] directly
```
Passing slice to operation affects the original register. This is the intended behavior for partitioning register into sub-registers, but can be surprising.

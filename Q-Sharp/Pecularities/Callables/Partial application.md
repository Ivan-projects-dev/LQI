#Q-Sharp
**Partial application** creates new **callable** by fixing some arguments of existing one, leaving the rest open. 
`_` (underscore) placeholder marks unfixed arguments.
```csharp
let addOne = (x : Int) -> x + 1; // full lambda
let add = (a : Int, b : Int) -> a + b;
let addFive = add(5, _); // partial: fixes first arg
Message($"{addFive(3)}"); // 8
```
Partially applied callable has the type of the remaining arguments → return type. `add(5, _)` has type `Int -> Int`.

**[[Oracle]] construction** is the primary use case in Q#. [[Grover search]] expects [[Oracle]] `(Qubit[], Qubit) => Unit is Adj + Ctl`, but our marking op usually has extra classical args:
```csharp
operation MarkState(register : Qubit[], target : Qubit, x0 : Int) : Unit is Adj + Ctl {
    within {
        ApplyPauliFromBitString(PauliX, false, IntAsBoolArray(x0, Length(register)), register);
    } 
    apply {
        Controlled X(register, target);
    }
}

// Fix x0 = 7, leave register & target open
let oracle = MarkState(_, _, 7); // type: (Qubit[], Qubit) => Unit is Adj + Ctl
```
`[[Oracle]]` can now be passed directly to any operation expecting `(Qubit[], Qubit) => Unit`.

**Operation & func wrappers** can both be partially applied. [[Functors]] are preserved if the underlying callable declares them:
```csharp
operation ApplyGate(op : Qubit => Unit is Adj, q : Qubit) : Unit is Adj {
    op(q);
}
let applyH = ApplyGate(H, _); // type: Qubit => Unit is Adj
Adjoint applyH(myQubit); // valid - Adj preserved
```

**Multiple placeholders** can appear in one expression - all `_` positions remain open:
```csharp
operation CNOT2(ctrl : Qubit, tgt : Qubit) : Unit is Adj + Ctl { CNOT(ctrl, tgt); }

let applyToFixed = CNOT2(_, myTarget);   // fixes target, ctrl open
let applyFromFixed = CNOT2(myCtrl, _);  // fixes ctrl, target open
```

**Partial application vs lambda** - both produce new callable, but partial app preserves characteristics automatically:
```csharp
// Equivalent functionally - but partial app keeps is Adj + Ctl:
let op1 = MarkState(_, _, 7); // keeps Adj + Ctl
let op2 = (r : Qubit[], t : Qubit) => MarkState(r, t, 7); // must re-declare Adj + Ctl separately
```
Prefer `_` over lambdas when wrapping [[Oracle]]-like operations for [[Grover search]] or [[QPE]] to avoid losing functor support.

**`Std.Canon` combinators with partial app**
```csharp
// ApplyToEach(op, qs) applies op to each qubit
ApplyToEach(H, register); // H on every qubit
ApplyToEach(CNOT(ctrl, _), targets); // CNOT from fixed ctrl to each target
ApplyToEach(Rz(theta, _), register); // Rz with fixed angle to each qubit
```

**Iteration & mapping patterns**
```csharp
// Mapped applies a classical func to each element
let doubled = Mapped(x -> x * 2, [1, 2, 3, 4]); // [2, 4, 6, 8]

// Filtered keeps elements satisfying predicate
let positives = Filtered(x -> x > 0, [-1, 2, -3, 4]); // [2, 4]

// ForEach applies side-effecting func to each element (no return)
ForEach(Message, ["a", "b", "c"]);
```
Partial application composes well with [[Q-Sharp generics]] - `Mapped` & `Filtered` accept generic lambdas, so the `_` placeholder works across any `'T`.
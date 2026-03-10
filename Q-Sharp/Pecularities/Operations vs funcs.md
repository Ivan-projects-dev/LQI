#Q-Sharp
Q# distinguishes $2$ kinds of callables: **operations** & **funcs**. The distinction is fundamental - it determines what the callable can do, how it can be composed, & whether it can be inverted or controlled.

**Operation** is a quantum subroutine - it can allocate [[Qubits]], apply gates, perform measurements, & call other operations. Operations may have side effects on [[Quantum state]].
```csharp
operation ApplyCNOT(ctrl : Qubit, target : Qubit) : Unit {
    CNOT(ctrl, target);
}
```

Return type `Unit` means no classical output. Operations can return classical values too:
```csharp
operation PrepareAndMeasure(q : Qubit) : Result {
    H(q);
    return MResetZ(q);
}
```

**Characteristics** declare what [[Functors]] the operation supports:
```csharp
operation MyOp(q : Qubit) : Unit is Adj { ... } // supports Adjoint
operation MyOp(q : Qubit) : Unit is Ctl { ... } // supports Controlled
operation MyOp(q : Qubit) : Unit is Adj + Ctl { ... } // supports both
```
Without a characteristic declaration, the operation cannot be used with `Adjoint` or `Controlled`. See [[Functors]].

**Func** is a **pure classical subroutine** - no quantum operations, no qubit access, no side effects. Given the same inputs, always returns the same output. The compiler can use funcs in classical preprocessing & in constant-folding.
```csharp
function GroverIterationCount(n : Int, m : Int) : Int {
    return Floor(PI() / 4.0 * Sqrt(IntAsDouble(1 <<< n) / IntAsDouble(m)));
}
```

funcs use `->` in their type signature (vs `=>` for operations):
```csharp
let f : Int -> Double = IntAsDouble; // func type
let op : Qubit => Unit is Adj = H; // operation type
```
funcs **cannot** call operations. Operations **can** call funcs.

| Property             | funcn                | func        |
|---|---|---|
| [[Quantum state]] access | Yes                  | No          |
| Measurement          | Yes                  | No          |
| `Adjoint` functor    | If declared `is Adj` | N/A         |
| `Controlled` functor | If declared `is Ctl` | N/A         |
| Side effects         | Allowed              | Not allowed |
| Type arrow           | `=>`                 | `->`        |
| Call from func       | No                   | Yes         |
| Call from operation  | Yes                  | Yes         |

Both operations & funcs are first-class values & can be passed as arguments:
```csharp
// Pass operation as argument
operation ApplyTwice(op : Qubit => Unit is Adj, q : Qubit) : Unit is Adj {
    op(q);
    op(q);
}
ApplyTwice(H, myQubit);

// Partial app
let applyH = ApplyTwice(H, _);    // underscore _ creates partial app
```
`_` placeholder creates a **partially applied** callable - a new callable with the remaining arguments. Used extensively in [[Functors]] patterns & for passing oracles to Grover:

```csharp
let oracle = MarkExactState(_, _, 11);   // marks state 11, register & target still open
```

Q# supports inline operation & func literals:
```csharp
// Lambda operation
let flipIfOne = (q : Qubit) => { if M(q) == One { X(q); } };

// Lambda func
let double = (x : Int) -> x * 2;
```
Used in [[Grover search]] to define inline oracles without declaring separate named operations.

**`newtype` & struct wrappers**

Custom named types can wrap callable types, useful for type-safe [[Oracle]] passing:
```csharp
newtype Oracle = (Qubit[], Qubit) => Unit is Adj + Ctl;

operation RunSearch(oracle : Oracle, n : Int) : Result[] {
    return RunGroverSearch(n, 1, oracle!);   // unwrap with !
}
```
[[Oracle]]! unwraps the `newtype` to its underlying value.
#Q-Sharp
**Callable types** in Q# encode not just input/output types but also the quantum characteristics of an operation. Type system ensures misuse (e.g., passing non-adjointable op where `Adj` required) is caught at compile time.

| Kind | Arrow | Example |
|---|---|---|
| Function | `->` | `Int -> Double` |
| Operation (no funcs) | `=>` | `Qubit => Unit` |
| Operation + `Adj` | `=> ... is Adj` | `Qubit => Unit is Adj` |
| Operation + `Ctl` | `=> ... is Ctl` | `Qubit => Unit is Ctl` |
| Operation + both | `=> ... is Adj + Ctl` | `Qubit => Unit is Adj + Ctl` |

**Declaring callable variables**
```csharp
let f : Int -> Double = IntAsDouble;
let op : Qubit => Unit = X;
let adj : Qubit => Unit is Adj = H;
let ctl : Qubit => Unit is Ctl = X;
let full : Qubit => Unit is Adj + Ctl = H;
```

**Callable as argument** - characteristics constrain what can be passed in:
```csharp
operation ApplyAdjointTwice(op : Qubit => Unit is Adj, q : Qubit) : Unit {
    Adjoint op(q);
    Adjoint op(q);
}
ApplyAdjointTwice(H, q); // ok - H is Adj
ApplyAdjointTwice(M, q); // compile error - M is not Adj
```

**Multi-qubit operation types** - input type is tuple or array:
```csharp
let cnot : (Qubit, Qubit) => Unit is Adj + Ctl = CNOT;
let qft : Qubit[] => Unit is Adj + Ctl = QFT; // from Std.Canon
```

**Characteristics subtyping** - an operation with `Adj + Ctl` can be used anywhere `Adj`, `Ctl`, or neither is expected. Characteristics are additive, not exclusive:
```csharp
operation NeedsAdj(op : Qubit => Unit is Adj, q : Qubit) : Unit { Adjoint op(q); }
NeedsAdj(H, q); // ok - H has Adj + Ctl ⊇ Adj
```

**Generic callable types** - type parameter `'T` in callable signatures. See [[Q-Sharp generics]]:
```csharp
operation ApplyToFirst<'T>(op : 'T => Unit, arr : 'T[]) : Unit {
    op(arr[0]);
}
```

**Returning callables from ops/funcs** - enables factory pattern:
```csharp
function PowerOf(base : Int) : (Int -> Int) {
    return exp -> base ^ exp; // returns a func
}
let pow2 = PowerOf(2);
Message($"{pow2(10)}"); // 1024

// Returning an operation
function PhaseGate(theta : Double) : (Qubit => Unit is Adj + Ctl) {
    return Rz(theta, _); // partial app - Adj + Ctl preserved
}
let rz90 = PhaseGate(PI() / 2.0);
rz90(q);
Adjoint rz90(q); // valid
```

**`[[Oracle]]` newtype pattern** - wraps callable type in named type for type safety:
```csharp
newtype PhaseOracle = (Qubit[] => Unit is Adj + Ctl);

operation Grover(oracle : PhaseOracle, n : Int) : Result[] {
    let op = oracle!;   // unwrap to underlying op type
    // ...
}
```
Prevents accidentally passing wrong [[Oracle]] type to algorithm. `!` unwraps; `::` not needed for single-item newtypes.

**Callable type arithmetic: `Adj` propagation rules**

Compiler auto-derives `Adj`/`Ctl` for composed ops when:
- All constituent ops support required characteristic.
- No measurements inside body (see [[Q-Sharp quirks]]).
- For `Adj`: body uses only `Adj`-supporting gates in `within/apply` or explicit `Adjoint` calls.
- For `Ctl`: body's ops all support `Ctl`; no classical-only branches conditioned on `Result`.

If auto-derivation fails, declare body explicitly with `adjoint` & `controlled` specializations:
```csharp
operation MyOp(q : Qubit) : Unit is Adj + Ctl {
    body (...) { 
	    H(q); 
	    T(q); 
	}
    adjoint (...) {
	    Adjoint T(q); 
	    Adjoint H(q); 
	}  // explicit adjoint
    controlled (ctrls, ...) {
	    Controlled H(ctrls, q);
	    Controlled T(ctrls, q); 
	}
    controlled adjoint (ctrls, ...) { ... }
}
```
Explicit specializations override auto-generation and allow custom decompositions (useful for optimized circuits).

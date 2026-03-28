#Q-Sharp
**Higher-order operations** take other operations as arguments or return them. Core tool for building reusable quantum primitives, [[Oracle]] factories, & composable circuit blocks. Primarily from `Std.Canon`.

**`ApplyIf` & `ApplyIfOne`** - conditionally apply op based on classical `Bool` or `Result`:
```csharp
// ApplyIf : (Bool, 'T => Unit, 'T) => Unit
ApplyIf(flag, X, q); // apply X to q only if flag is true

// ApplyIfOne : (Result, Qubit => Unit, Qubit) => Unit
ApplyIfOne(M(ctrl), X, target);  // apply X to target if ctrl measured One
```
`ApplyIfZero`, `ApplyIfOneA`, `ApplyIfOneCA` variants preserve `Adj`/`Ctl` characteristics.

**`BoundCA` / `Bound`** - compose list of operations into single operation applied sequentially:
```csharp
// BoundCA : ('T => Unit is Adj + Ctl)[] -> ('T => Unit is Adj + Ctl)
let prepBell = BoundCA([H, CNOT(_, partner)]); // H then CNOT
prepBell(q); // executes H(q); CNOT(q, partner)
```
Useful for building parameterized multi-step circuits from component ops. `BoundA` preserves `Adj` only; `Bound` preserves neither.

**`ControlledOnBitString`** - multi-[[Controlled op]] where each control qubit matches a specific bit value (not just `|1...1⟩`):
```csharp
// Apply X to target iff control register encodes 0b101 = 5
ControlledOnBitString([true, false, true], X)(controls, target);
```
Equivalent to flipping controls matching `0` bits, applying `Controlled X`, then flipping back. Replaces verbose manual `within { X(q); } apply { Controlled X(...) }` patterns.

**`ControlledOnInt`** - controlled on integer value. Convenience wrapper over `ControlledOnBitString`:
```csharp
// Apply Rz to target iff register == 3
ControlledOnInt(3, Rz(theta, _))(register, target);
```

**`ApplyWithInputTransformation`** - apply op in transformed input space. Useful when op expects different qubit ordering:
```csharp
// Swap arguments before passing to CNOT
ApplyWithInputTransformation(SWAP, CNOT)(q1, q2);
```

**`Delay`** - wrap op & its args into a zero-arg callable (thunk), deferring execution:
```csharp
// Delay : (('T => Unit), 'T) -> (() => Unit)
let deferred = Delay(H, q);
// ... later:
deferred(); // H(q) executes now
```
Used for building conditional branches where op & args are bundled before a runtime decision.

**`ApplyCNOTChain`** - [[CNOT]] ladder across qubit array, each qubit controlled by the previous:
```csharp
ApplyCNOTChain(register); // CNOT(0,1); CNOT(1,2); ... CNOT(n-2, n-1)
```
Common in [[Bell states]] preparation & [[Quantum Fourier Transform]] subroutines.

**`CCA` - Controlled-Controlled-A**: applies doubly-controlled version of any adjointable op:
```csharp
CCA(H)(ctrl1, ctrl2, target); // Toffoli-like: H if both ctrls are |1⟩
```

**[[Oracle]] factories** - pattern for building parametric oracles at runtime using higher-order ops:
```csharp
function BuildPhaseOracle(x0 : Int) : (Qubit[] => Unit is Adj + Ctl) {
    return register => {
        within {
            ApplyPauliFromBitString(PauliX, false, IntAsBoolArray(x0, Length(register)), register);
        } 
        apply {
            use anc = Qubit();
            within { 
	            X(anc); 
	            H(anc); 
	        }
            apply { 
	            Controlled X(register, anc); 
	            }
        }
    };
}

let oracle = BuildPhaseOracle(7);
oracle(qReg);    // phases |7⟩
```
Factory funcs return operation values with full `Adj + Ctl` support; compiler infers characteristics from the lambda body.

**`Std.Canon` reference: key higher-order ops**

| Callable | Signature | Purpose |
|---|---|---|
| `BoundCA` | `('T => Unit is Adj+Ctl)[]` → same | Sequential composition |
| `ApplyIf` | `(Bool, op, arg)` | Classical conditional |
| `ApplyIfOne` | `(Result, op, arg)` | Result-conditional |
| `ControlledOnInt` | `(Int, op)(ctrls, tgt)` | Control on integer |
| `ControlledOnBitString` | `(Bool[], op)(ctrls, tgt)` | Control on bit pattern |
| `CCA` | `op → doubly-[[[[[[Controlled op]]]]]]` | Double-control wrapper |
| `ApplyCNOTChain` | `Qubit[] → Unit` | [[CNOT]] ladder |
| `Delay` | `(op, arg) → () → Unit` | Deferred execution |

See [[Partial application]] for `_` syntax used with these combinators. See [[Within-Apply pattern]] for how `BoundCA` composes with auto-uncomputation.

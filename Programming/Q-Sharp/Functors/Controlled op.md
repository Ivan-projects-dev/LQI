#Quantum #Q-Sharp 
`Controlled op(controls, target)` applies `op` to `target` only when all [[Qubits]] in `controls` are $|1\rangle$.
Controlled-H: apply $H$ to $q$ only if ctrl $== |1⟩$ `Controlled H([ctrl], q);`
Multi-controlled: apply $X$ to $q$ only if all of $[c0, c1, c2] == |1⟩$ `Controlled X([c0, c1, c2], q);`

Operation must declare `is Ctl` to support `Controlled`. Signature for full support of both:
`operation MyOp(q : Qubit) : Unit is Adj + Ctl { ... }`

`Controlled` is how [[QPE]] implements $C$-$U^{2^k}$: each control qubit applies controlled version of the unitary to the eigenstate register.

[[Functors]] compose: `Controlled Adjoint` or `Adjoint Controlled op` (both valid, equivalent for unitaries).
`Controlled Adjoint T([ctrl], q);` Controlled-Adjoint of T gate

**`ControlledOnInt`** - applies operation controlled on a specific int state of a qubit register (not just all-$|1\rangle$):
```csharp
import Std.Canon.*;
// Apply X to target only if register == 5 (binary 101)
ControlledOnInt(5, X)(register, target);
```
Useful for [[Oracle]] construction - marks specific computational basis states.

**`ControlledOnBitString`** - same but controlled on explicit bit pattern:
```csharp
import Std.Canon.*;
ControlledOnBitString([true, false, true], X)(register, target);
```

**Controlled generation strategies** — declared alongside adjoint strategies in the operation body:

| Strategy | Declaration | When to use |
|---|---|---|
| `controlled auto` | `controlled auto;` | Compiler wraps each gate in the body with a `Controlled` version automatically. Works when all inner ops support `Ctl`. |
| `controlled distribute` | `controlled distribute;` | Distributes the control qubit array into nested controlled calls. Needed for operations whose bodies call other controlled operations. |

```csharp
operation MyRotation(angle : Double, q : Qubit) : Unit is Adj + Ctl {
    body { Rz(angle, q); }
    adjoint auto;
    controlled auto;            // auto wraps Rz → Controlled Rz
    controlled adjoint auto;    // auto wraps Adjoint Rz → Controlled Adjoint Rz
}
```

When an operation is declared `is Adj + Ctl`, Q# requires all four specializations to be satisfiable: `body`, `adjoint`, `controlled`, `controlled adjoint`. Using `auto` for all is the most common pattern; manual bodies are only needed for performance-critical decompositions.

## Sources
- [Functor application in Q#](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/expressions/functorapplication)
- [Controlled functor](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/typesystem/operationsandfunctions)
- [Multi-qubit gates (Std.Intrinsic)](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.intrinsic)

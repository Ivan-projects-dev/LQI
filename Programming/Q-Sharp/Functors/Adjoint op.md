#Q-Sharp #Quantum 
`Adjoint op` produces the adjoint (conjugate transpose / inverse) of `op`. For unitary operations, the adjoint is the operation that "undoes" the original.
```csharp
operation ApplyH(q : Qubit) : Unit is Adj {
    H(q);
}

H(q);
Adjoint H(q);  // undoes H - back to original state
```
Operation must declare `is Adj` (or `is Adj + Ctl`) in its signature to support `Adjoint`. Q# can often auto-generate the adjoint by reversing gate sequence & taking complex conjugates - `auto` body.
```csharp
operation MyCircuit(q : Qubit) : Unit is Adj {
    body { H(q); T(q); CNOT(q, r); }
    adjoint auto;  // auto-reversed: Adjoint CNOT; Adjoint T; H
}
```
Used heavily for **uncomputation**: apply operation, use its side-effect, then `Adjoint` it to clean [[Ancilla]] [[Qubits]].

**Adjoint generation strategies** — declared in the operation body:

| Strategy | Declaration | When to use |
|---|---|---|
| `adjoint auto` | `adjoint auto;` | Compiler reverses gate sequence & conjugates phases automatically. Works when body uses only adjointable ops. |
| `adjoint self` | `adjoint self;` | Operation is its own inverse (self-adjoint / Hermitian). Applies to H, X, Y, Z, [[CNOT]] — `Adjoint H == H`. |
| `adjoint invert` | `adjoint invert;` | Same as `auto` — synonym used in older Q# syntax. |
| `adjoint distribute` | `adjoint distribute;` | Used when generating `Controlled Adjoint`: distributes the adjoint into the controlled body. Needed when `controlled auto` alone is insufficient. |

```csharp
operation HermitianGate(q : Qubit) : Unit is Adj {
    body { H(q); }
    adjoint self;   // H is self-adjoint: Adjoint H == H
}

operation CustomCircuit(qs : Qubit[]) : Unit is Adj + Ctl {
    body { H(qs[0]); CNOT(qs[0], qs[1]); T(qs[1]); }
    adjoint auto;      // auto-generates: Adjoint T; Adjoint CNOT; H
    controlled auto;   // auto-generates controlled version
    controlled adjoint auto;  // auto-generates controlled adjoint
}
```

## Sources
- [Functor application in Q#](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/expressions/functorapplication)
- [Adjoint functor & reversibility](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/typesystem/operationsandfunctions)
- [Std.Intrinsic API reference](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.intrinsic)

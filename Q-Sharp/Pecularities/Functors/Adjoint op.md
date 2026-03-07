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
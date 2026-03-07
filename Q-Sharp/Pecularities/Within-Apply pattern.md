#Q-Sharp #Quantum
**`within { } apply { }`** is Q# control flow construct for the **compute-action-uncompute** pattern, essential in quantum [[Oracle]] construction & diffusion operators.

Pattern safely applies operation in transformed basis & automatically uncomputes transformation:
```csharp
within {
    // Forward transformation V
} apply {
    // Core operation O (runs in V-transformed basis)
}
// Q# automatically runs Adjoint V here
```
Equivalent to manually writing: `V; O; Adjoint V` - but `within/apply` is safer (Q# guarantees uncomputation even if `O` throws) & $>$ readable.

**Ancilla [[Qubits]]** borrowed for intermediate computation must be returned to $|0\rangle$ before release - otherwise they cause entanglement pollution. `within/apply` enforces this structurally.

**Diffusion operator** $D = H^{\otimes n}(2|0\rangle\langle 0| - I)H^{\otimes n}$ is canonical example:
```csharp
operation ApplyDiffusion(register : Qubit[]) : Unit is Adj + Ctl {
    within {
        ApplyToEachA(H, register);        // transform to |s⟩ basis
        ApplyToEachA(X, register);        // map |0⟩ → |1...1⟩
    } apply {
        Controlled Z(register[1...], register[0]);  // phase flip |1...1⟩
    }
    // Q# auto-applies: ApplyToEachA(X,...) then ApplyToEachA(H,...)
}
```

**[[Oracle]] uncomputation example**

Marking [[Oracle]] for $f(x) = 1$ iff $x = x_0$: compute into ancilla, flip target, uncompute:
```csharp
operation MarkSolution(
    register : Qubit[],
    target   : Qubit,
    x0       : Int
) : Unit is Adj + Ctl {
    within {
        // Flip qubits that are 0 in x0 (so all-|1⟩ = x0)
        ApplyPauliFromBitString(PauliX, false, IntAsBoolArray(x0, Length(register)), register);
    } apply {
        Controlled X(register, target);   // flip target iff register == x0
    }
}
```
 `within` block is auto-adjoint after `apply`, restoring `register` to its original state.

`ApplyToEach(op, qs)` - applies `op` to every qubit in array `qs`.
`ApplyToEachA(op, qs)` - same, but `op` must support `Adjoint` (needed inside `within`).
`ApplyToEachCA(op, qs)` - `op` supports both `Adj + Ctl`.
```csharp
ApplyToEach(H, qubits);           // H⊗n
ApplyToEachA(X, qubits);          // X⊗n (adjointable)
```
These replace explicit `for` loops over qubit arrays & are idiomatic Q#.

**`within/apply` vs manual [[Adjoint op]]**

| Approach                   | Uncomputation | Error-safe | Readable |
| -------------------------- | ------------- | ---------- | -------- |
| Manual `V; O; Adjoint V`   | Explicit      | No         | Low      |
| `within { V } apply { O }` | Automatic     | Yes        | High     |

Always prefer `within/apply` when `V` & its adjoint bracket core operation `O`.

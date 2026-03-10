#Quantum #Q-Sharp [[Functors]]
`Controlled op(controls, target)` applies `op` to `target` only when all [[Qubits]] in `controls` are $|1\rangle$.
```csharp
// Controlled-H: apply H to q only if ctrl == |1⟩
Controlled H([ctrl], q);

// Multi-controlled: apply X to q only if all of [c0, c1, c2] == |1⟩
Controlled X([c0, c1, c2], q);
```

Operation must declare `is Ctl` to support `Controlled`. Signature for full support of both:
```csharp
operation MyOp(q : Qubit) : Unit is Adj + Ctl { ... }
```

`Controlled` is how [[QPE]] implements $C$-$U^{2^k}$: each control qubit applies controlled version of the unitary to the eigenstate register.

[[Functors]] compose: `Controlled Adjoint` or `Adjoint Controlled op` (both valid, equivalent for unitaries).
```csharp
// Controlled-Adjoint of T gate
Controlled Adjoint T([ctrl], q);
```

**`ControlledOnInt`** - applies operation controlled on a specific int state of a qubit register (not just all-$|1\rangle$):
```csharp
// Apply X to target only if register == 5 (binary 101)
ControlledOnInt(5, X)(register, target);
```
Useful for [[Oracle]] construction - marks specific computational basis states.

**`ControlledOnBitString`** - same but controlled on explicit bit pattern:
```csharp
ControlledOnBitString([true, false, true], X)(register, target);
```

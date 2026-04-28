#Q-Sharp #Algorithm
Arithmetic oracles in Q# - encode numerical conditions as quantum marking operations. Used in [[Bounded knapsack oracle]], [[Grover]]-based optimization, & quantum search on structured data.
1. [[Equality Oracle]]
2. [[Inequality Oracle]]
3. [[Greater-than Oracle]]
4. [[Range Oracle]]
5. [[Divisibility Oracle]]
6. [[Weighted Sum Threshold Oracle]]

## Parity of Integer (odd/even)

Flip target iff int in `register` is odd (LSB is $1$):
```csharp
operation MarkOdd(register : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    CNOT(register[0], target); // register[0] = LSB in little-endian encoding
}
```

**All arithmetic oracles must be `Adj + Ctl`** - they will be called inside `within/apply` blocks or as controlled operations in larger oracles. Any non-reversible step (measurement) inside an [[Oracle]] breaks adjointability.

**[[Ancilla]] registers must be uncomputed.** Every scratch register allocated with `use` inside the `within` block of a `within/apply` pattern is automatically uncomputed by the adjoint of the within block. Never leave [[Ancilla]] in a non-$|0\rangle$ state.

**Little-endian convention.** Q# `Std.Arithmetic` uses little-endian register encoding by default: `register[0]` is the least significant bit. `ControlledOnInt` uses the same convention.

## Sources
- [Std.Arithmetic API reference](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.arithmetic)
- [Q# integer representation (little-endian)](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/typesystem/datatypes)
- [ControlledOnInt documentation](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.canon/controlledonint)
- [Quantum arithmetic (Wikipedia)](https://en.wikipedia.org/wiki/Quantum_arithmetic)
- [Bounded knapsack oracle](https://learn.microsoft.com/en-us/azure/quantum/tutorial-qdk-grovers-search)

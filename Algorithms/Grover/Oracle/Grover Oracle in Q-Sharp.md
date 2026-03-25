#Q-Sharp #Quantum #Algorithm
[[Oracle]] implementation in Q# for [[Grover]]'s search. In Q#, you typically write the **marking [[Oracle]]** & convert it to phase [[Oracle]] via phase kickback.

**Marking [[Oracle]] signature**
```csharp
operation MarkingOracle(
    register : Qubit[],   // n-qubit input encoding search space
    target   : Qubit      // ancilla: flipped iff f(register) = 1
) : Unit is Adj + Ctl     // must support Adjoint for uncomputation
```
`is Adj + Ctl` is mandatory - [[Grover]]'s diffusion uses `Adjoint` internally & [[QPE]] uses [[Controlled op]]

**Converting marking → phase [[Oracle]]**

Phase [[Oracle]] applies $(-1)^{f(x)}$ to $|x\rangle$ via phase kickback from target in $|{-}\rangle$:
```csharp
operation ApplyMarkingAsPhase(
    markingOracle : (Qubit[], Qubit) => Unit is Adj,
    register      : Qubit[]
) : Unit is Adj {
    use target = Qubit();
    within {
        X(target);
        H(target);               // prepare |−⟩ = X then H applied to |0⟩
    } apply {
        markingOracle(register, target);   // phase kickback happens here
    }
    // auto-uncompute: H then X → restores target to |0⟩
}
```

**Hardcoded single-solution [[Oracle]]** (marks state $|x_0\rangle$)

Uses `ControlledOnInt` to flip target iff register == $x_0$:
```csharp
operation MarkExactState(
    register : Qubit[],
    target   : Qubit,
    x0       : Int
) : Unit is Adj + Ctl {
    ControlledOnInt(x0, X)(register, target);
}
```
`ControlledOnInt` handles the $|0\rangle$-qubit flipping internally - no manual `within/apply` needed.

**Multi-solution [[Oracle]]** (marks a list of solutions)
```csharp
operation MarkMultipleSolutions(
    register  : Qubit[],
    target    : Qubit,
    solutions : Int[]
) : Unit is Adj + Ctl {
    for sol in solutions {
        ControlledOnInt(sol, X)(register, target);
    }
}
```
Each `ControlledOnInt` contributes independently - valid because solutions are distinct basis states.

**SAT [[Oracle]] structure in Q#**

Each clause is marking [[Oracle]]. Combine with [[Ancilla]]-per-clause + `within/apply`:
```csharp
operation MarkSAT(
    register : Qubit[],
    target   : Qubit,
    clauses  : (Int[], Bool[])[]   // each clause: (variable indices, negation flags)
) : Unit is Adj + Ctl {
    use clauseAncilla = Qubit[Length(clauses)];
    within {
        for (i, (vars, negs)) in Enumerated(clauses) {
            MarkClause(register, clauseAncilla[i], vars, negs);
        }
    } apply {
        // target flipped iff all clauses satisfied (all ancilla == |1⟩)
        Controlled X(clauseAncilla, target);
    }
}

operation MarkClause(
    register : Qubit[],
    target   : Qubit,
    varIdx   : Int[],
    negated  : Bool[]
) : Unit is Adj + Ctl {
    // Clause is FALSE iff all literals are false → mark that case, flip
    within {
        for (i, idx) in Enumerated(varIdx) {
            if negated[i] { X(register[idx]); }   // negate literal
        }
    } apply {
        // If all literals false → all relevant qubits are |0⟩ after negation
        // We mark (NOT clause) - so clause satisfied = ancilla NOT flipped
        // Adjust: flip target when clause is satisfied (at least $1$ |1⟩)
        // Implementation: use NOR → flip all, multi-CNOT, flip all back
        ApplyToEachA(X, register[varIdx]);
        Controlled X(register[varIdx], target);   // target = 1 iff clause FALSE
        ApplyToEachA(X, register[varIdx]);
    }
    // within auto-uncomputes negation
    // Final target: |1⟩ = clause violated; Grover marks violations, inverts logic at top level
}
```

`Adj` support is required - the [[Within-Apply pattern]] in the [[Diffusion operator]] & [[Oracle]] uncomputation both call Adjoint op. Operations that contain measurements (`M`, `Measure`) cannot be `Adjoint`-able - avoid measurements inside oracles.

[[Ancilla]] [[Qubits]] allocated with `use` inside an operation are automatically reset to $|0\rangle$ on scope exit only if dev does so explicitly or via `within/apply`. Leaving [[Ancilla]] in non-$|0\rangle$ state causes aruntime error.

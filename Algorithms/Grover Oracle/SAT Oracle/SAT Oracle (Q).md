#Q-Sharp #Algorithm Example of [[SAT oracle]]
$3$ variables, $3$ clauses:
$$\phi = (x_0 \vee x_1 \vee x_2) \wedge (\neg x_0 \vee \neg x_1 \vee x_2) \wedge (x_0 \vee \neg x_1 \vee \neg x_2)$$
Satisfying assignments: `001`, `010`, `100`, `101`, `111` - $5$ out of $8$.
```csharp
import Std.Arrays.*;
import Std.Canon.*;

// Mark ancilla = |1⟩ when a clause is VIOLATED
// (i.e., all literals in the clause are false)
// positiveVars: indices of variables appearing positive (x_i)
// negativeVars: indices of variables appearing negated (¬x_i)
operation MarkClauseViolation(register : Qubit[],
    ancilla : Qubit, positiveVars : Int[],
    negativeVars : Int[]) : Unit is Adj + Ctl {
    // Clause violated when all positive vars = 0 AND all negative vars = 1
    // Flip positive vars so that 0 → 1 ("X before controlled-X"),
    // then detect all-ones with multi-controlled X.
    within {
        for i in positiveVars { X(register[i]); }
    }
    apply {
        let controls = Mapped(i -> register[i], positiveVars + negativeVars);
        Controlled X(controls, ancilla);
    }
}

// Marking oracle: flip target iff the 3-SAT formula is satisfied
operation MarkSAT(register : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    // 3 clause ancillas (one per clause)
    use clauseAnc = Qubit[3];

    within {
        // Clause 1: (x0 OR x1 OR x2) → violated when x0=0, x1=0, x2=0
        MarkClauseViolation(register, clauseAnc[0], [0, 1, 2], []);

        // Clause 2: (NOT x0 OR NOT x1 OR x2) → violated when x0=1, x1=1, x2=0
        MarkClauseViolation(register, clauseAnc[1], [2], [0, 1]);

        // Clause 3: (x0 OR NOT x1 OR NOT x2) → violated when x0=0, x1=1, x2=1
        MarkClauseViolation(register, clauseAnc[2], [0], [1, 2]);
    }
    apply {
        // Formula satisfied ↔ no clause is violated ↔ all clauseAnc = |0⟩
        // Flip target when all clauseAnc are 0: X-flip ancillas, multi-controlled X, unflip
        within { ApplyToEach(X, clauseAnc); }
        apply   { Controlled X(clauseAnc, target); }
    }
}

// Phase oracle: applies -1 phase to satisfying assignments
// Uses phase kickback: prepare target as |−⟩ before calling MarkSAT
operation PhaseSAT(register : Qubit[]) : Unit is Adj {
    use target = Qubit();
    within { X(target); H(target); }  // |0⟩ → |−⟩
    apply  { MarkSAT(register, target); }
}

// Full Grover search for the SAT instance (n=3 variables)
operation SolveWith3SAT() : Result[] {
    let n = 3;
    // 5 solutions out of 8 → optimal iteration count ≈ 1
    let iterations = 1;

    use register = Qubit[n];
    ApplyToEach(H, register); // uniform superposition

    for _ in 1..iterations {
        PhaseSAT(register); // phase oracle
        ReflectAboutUniform(register); // diffusion operator (Std.Grover)
    }

    return MResetEach(register);
}
```

`MarkClauseViolation` uses 
```csharp
within {
	X(positive vars) 
} apply { 
	multi-controlled X 
}
``` 
  to detect the all-false pattern without permanently altering the register. `within/apply` conjugation auto-generates the adjoint for uncomputation.
- Outer `within` in `MarkSAT` computes all clause violation flags & automatically uncomputes them after the `apply` block - no manual `Adjoint` calls needed.
- `PhaseSAT` wraps the marking [[Oracle]] via [[Phase kickback]]: target prepared as $|{-}\rangle$ turns a marking flip into a $(-1)$ phase on $|x\rangle$.
- `ReflectAboutUniform` is Q#'s built-in [[Diffusion operator]] from `Std. `[[Grover]] (or `Std.Canon` depending on version).

**Extension to general CNF:** Replace the hard-coded clause list with an array of `(Int[], Int[])` tuples (positive vars, negative vars) & loop over them:
```csharp
operation MarkGeneralCNF(register : Qubit[], target : Qubit,
    clauses : (Int[], Int[])[]) : Unit is Adj + Ctl {
    use clauseAnc = Qubit[Length(clauses)];
    within {
        for (i, (pos, neg)) in Enumerated(clauses) {
            MarkClauseViolation(register, clauseAnc[i], pos, neg);
        }
    }
    apply {
        within { ApplyToEach(X, clauseAnc); }
        apply  { Controlled X(clauseAnc, target); }
    }
}
```
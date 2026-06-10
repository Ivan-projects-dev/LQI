#Q-Sharp #Algorithm Graph coloring [[Oracle]]
**Concrete example:** Triangle graph ($3$ vertices, $3$ edges), $K = 3$ colors, each vertex encoded in $2$ [[Qubits]].

Register layout: `[v0_b0, v0_b1, v1_b0, v1_b1, v2_b0, v2_b1]` - $6$ [[Qubits]] total.
Edges: $(0,1)$, $(1,2)$, $(0,2)$.
```csharp
import Std.Arrays.*;
import Std.Canon.*;

// Mark ancilla = |1⟩ when two vertex color registers are EQUAL
// (i.e., the edge constraint is violated - same color on both endpoints)
operation MarkEdgeConflict(
    colorU : Qubit[], // qubits encoding vertex u's color
    colorV : Qubit[], // qubits encoding vertex v's color
    ancilla : Qubit  // flipped if colorU == colorV
) : Unit is Adj + Ctl {
    let bits = Length(colorU);
    use xorAnc = Qubit[bits];

    within {
        // xorAnc[i] = colorU[i] XOR colorV[i]
        // = 0 when bits agree, 1 when they differ
        for i in 0..bits-1 {
            CNOT(colorU[i], xorAnc[i]);
            CNOT(colorV[i], xorAnc[i]);
        }
        // Colors equal ↔ all xor bits = 0
        // Flip xorAnc so that 0 → 1 (equal bit → 1)
        ApplyToEach(X, xorAnc);
    }
    apply {
        // ancilla = 1 iff all xorAnc = 1 ↔ all bits agreed ↔ colors equal
        Controlled X(xorAnc, ancilla);
    }
}

// Marking oracle: flip target iff the coloring is VALID
// (no two adjacent vertices share a color)
operation MarkValidColoring(register : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    // Split register into per-vertex color sub-registers (2 qubits each)
    let bitsPerColor = 2;
    let v0 = register[0..1];
    let v1 = register[2..3];
    let v2 = register[4..5];

    // One ancilla per edge: flipped when that edge has a conflict
    use edgeAnc = Qubit[3];

    within {
        MarkEdgeConflict(v0, v1, edgeAnc[0]);   // edge (0,1)
        MarkEdgeConflict(v1, v2, edgeAnc[1]);   // edge (1,2)
        MarkEdgeConflict(v0, v2, edgeAnc[2]);   // edge (0,2)
    }
    apply {
        // Coloring valid ↔ no conflict ↔ all edgeAnc = |0⟩
        // Flip target when all edgeAnc are 0
        within { ApplyToEach(X, edgeAnc); }
        apply  { Controlled X(edgeAnc, target); }
    }
}

// Phase oracle wrapper
operation PhaseColorOracle(register : Qubit[]) : Unit is Adj {
    use target = Qubit();
    within { X(target); H(target); }
    apply  { MarkValidColoring(register, target); }
}

// Full Grover search for 3-coloring of the triangle
// Triangle has exactly 6 valid 3-colorings out of 3^3 = 27 total
// Optimal iterations ≈ round(π/4 * sqrt(27/6)) ≈ 1
operation Find3Coloring() : Result[] {
    let n = 6;   // 3 vertices × 2 bits per color
    use register = Qubit[n];
    ApplyToEach(H, register);   // uniform superposition over all 2^6 = 64 states
    // Note: some states encode colors ≥ 3 (invalid encodings); the oracle
    // ignores them since no coloring assignment will mark them as valid.

    for _ in 1..2 {
        PhaseColorOracle(register);
        ReflectAboutUniform(register);
    }

    return MResetEach(register);
}
```
**Reading the result:** output `Result[]` of length $6$ encodes `[v0_b0, v0_b1, v1_b0, v1_b1, v2_b0, v2_b1]`. Interpret each pair as a 2-bit int (color $0$, $1$, or $2$). For example, `[Zero, One, One, Zero, Zero, Zero]` $→$ colors $(1, 2, 0)$ - all different on the triangle, a valid $3$-coloring.

**Edge case - invalid color encodings:** With $2$ [[Qubits]] & $K=3$ colors, the state $|11\rangle$ (decimal $3$) is invalid color. [[Oracle]] marks valid colorings only, so the [[Grover]] amplitude amplification correctly ignores invalid encodings. For $K = 4$ (exact power of $2$), invalid states disappear entirely.

Source: [microsoft/qsharp - Grover.qs](https://github.com/microsoft/qsharp/blob/main/samples/algorithms/Grover.qs) · [Microsoft Learn - Grover's algorithm tutorial](https://learn.microsoft.com/en-us/azure/quantum/tutorial-qdk-grovers-search)

**Extension to arbitrary graphs:**
```csharp
operation MarkValidColoringGeneral(
    register : Qubit[],
    edges : (Int, Int)[], // list of (u, v) edge pairs
    bitsPerColor : Int // ceil(log2(K))
) : Unit is Adj + Ctl {
    use edgeAnc = Qubit[Length(edges)];
    within {
        for (i, (u, v)) in Enumerated(edges) {
            let colorU = register[u * bitsPerColor..(u+1) * bitsPerColor - 1];
            let colorV = register[v * bitsPerColor..(v+1) * bitsPerColor - 1];
            MarkEdgeConflict(colorU, colorV, edgeAnc[i]);
        }
    }
    apply {
        within { ApplyToEach(X, edgeAnc); }
        apply  { Controlled X(edgeAnc, target); }
    }
}
```
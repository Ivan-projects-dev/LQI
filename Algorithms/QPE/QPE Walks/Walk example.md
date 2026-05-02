#Algorithm #Q-Sharp [[QPE]] Walks
Walk on $4$-cycle
Graph: $0 \to 1 \to 2 \to 3 \to 0$ (symmetric). Classical walk [[Matrix]]:
$$P = \frac{1}{2}\begin{pmatrix}0&1&0&1\\1&0&1&0\\0&1&0&1\\1&0&1&0\end{pmatrix}$$
Eigenvalues of $P$: $1, 0, -1, 0$. Walk angles: $\theta \in \{0, \pi/2, \pi\}$.
[[QPE]] on the walk operator $W$ returns these phases. From $\theta = \pi/2$ (non-trivial eigenvalue): eigenvalue of $P$ is $\cos(\pi/2) = 0$ - the walk mixes in $O(N)$ steps.
### Szegedy walk construction
For graph with transition [[Matrix]] $P_{ij}$, the Szegedy walk acts on the edge space $\mathbb{C}^N \otimes \mathbb{C}^N$:
$$|i\rangle|p_i\rangle = |i\rangle\sum_j\sqrt{P_{ij}}|j\rangle$$
**Walk operator**: $W = S \cdot (2\Pi - I)$ where:
- $S|i\rangle|j\rangle = |j\rangle|i\rangle$ - [[SWAP]] of edge endpoints
- $\Pi = \sum_i |i\rangle\langle i| \otimes |p_i\rangle\langle p_i|$ - projection onto edge states
### Continuous-time walk (graph Laplacian)
For $4$-node cycle, the Laplacian $L$ has eigenvalues $0, 1, 2, 2$ (rescaled).
```csharp
import Std.Math.*;
import Std.Convert.*;
import Std.Measurement.*;

// Cycle-4 Laplacian evolution (2-qubit system)
// Approximate L as sum of 2-local Pauli terms: L ≈ I - (XX + YY + ZZ)/2
// For toy: use the diagonal walk on the complete graph K4 (easier to implement)
// K4 walk: P = (J - I)/3 where J = all-ones. Eigenvalues: 1 (once), -1/3 (three times)
// Walk operator eigenphases: 0 & arccos(-1/3) ≈ 0.3926π
operation CycleWalkStep(register : Qubit[]) : Unit is Adj + Ctl {
    // 4-node cycle: superposition over neighbours
    // Encode node 0=|00⟩, 1=|01⟩, 2=|10⟩, 3=|11⟩
    // Step: |i⟩ → (|i-1 mod 4⟩ + |i+1 mod 4⟩)/√2
    // Implemented as QFT → phase → QFT†
    ApplyQFT(register);
    // Phase corresponding to cos(2πk/4): momentum k eigenphase
    R1(PI() / 2.0, register[0]);        // k=1 component
    Controlled R1([register[1]], (PI(), register[0])); // k=2 phase
    R1(3.0 * PI() / 2.0, register[0]);  // k=3 component
    Adjoint ApplyQFT(register);
}

operation EstimateWalkSpectrum(nClock : Int) : Double {
    use (clock, node) = (Qubit[nClock], Qubit[2]);
    // Prepare uniform superposition over nodes
    ApplyToEach(H, node);
    // QPE on walk step
    ApplyToEach(H, clock);
    for j in 0..nClock - 1 {
        for _ in 1..1 <<< j {
            Controlled CycleWalkStep([clock[j]], node);
        }
    }
    Adjoint ApplyQFT(clock);
    let bits = MResetEachZ(clock);
    let phaseInt = ResultArrayAsIntBE(bits);
    let phi = IntAsDouble(phaseInt) / IntAsDouble(1 <<< nClock);
    ResetAll(node);
    // phi ≈ 0: uniform (eigenvalue 1), phi ≈ 0.5: eigenvalue -1 (cycle gap)
    return phi;
}
```

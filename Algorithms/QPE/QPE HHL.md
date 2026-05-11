#Algorithm #Math
**[[HHL]] algorithm** (Harrow-Hassidim-Lloyd, $2009$) solves linear system $A\vec{x} = \vec{b}$ exponentially faster than classical Gaussian elimination for certain sparse, well-conditioned matrices. [[QPE]] is its core subroutine: it extracts the eigenvalues of $A$ so they can be inverted quantumly.

**Input**: $n\times n$ Hermitian [[Matrix]] $A$ (eigenvalues $\lambda_j$), [[Vector]] $\vec{b}$ encoded as $|b\rangle$.
**Output**: [[Quantum state]] $|x\rangle \propto A^{-1}|b\rangle$.

**Decompose**: $|b\rangle = \sum_j \beta_j |u_j\rangle$ where $A|u_j\rangle = \lambda_j|u_j\rangle$.
Then $A^{-1}|b\rangle = \sum_j \frac{\beta_j}{\lambda_j}|u_j\rangle$.
**$3$ stages:**
1. **[[QPE]]**: extract eigenvalues into a clock register $$\sum_j \beta_j |u_j\rangle|0\rangle \xrightarrow{\text{Q}} \sum_j \beta_j |u_j\rangle|\tilde\lambda_j\rangle$$where $Q$ - [[QPE]]
2. **Controlled rotation**: [[Ancilla]] qubit rotated by $C/\tilde\lambda_j$$$\sum_j \beta_j |u_j\rangle|\tilde\lambda_j\rangle\left(\sqrt{1 - \frac{C^2}{\tilde\lambda_j^2}}|0\rangle + \frac{C}{\tilde\lambda_j}|1\rangle\right)$$
3. **Inverse [[QPE]]**: uncompute the clock register (within/apply pattern)
   Post-select [[Ancilla]] $= |1\rangle$: obtain $\sum_j \frac{\beta_j C}{\lambda_j}|u_j\rangle = C\cdot A^{-1}|b\rangle$ ✓
### $2×2$ system
$$A = \begin{pmatrix}1.5 & 0.5 \\ 0.5 & 1.5\end{pmatrix}, \quad \vec{b} = \begin{pmatrix}1 \\ 0\end{pmatrix}$$
**Eigenvalues**: $\lambda_1 = 1$, $\lambda_2 = 2$
**Eigenvectors**: $|u_1\rangle = (|0\rangle - |1\rangle)/\sqrt2$, $|u_2\rangle = (|0\rangle + |1\rangle)/\sqrt2$

Since $|u_1\rangle = (|0\rangle - |1\rangle)/\sqrt2$ and $|u_2\rangle = (|0\rangle + |1\rangle)/\sqrt2$, we get $|b\rangle = |0\rangle = \tfrac{1}{\sqrt2}|u_1\rangle + \tfrac{1}{\sqrt2}|u_2\rangle$ ($\beta_1 = \beta_2 = 1/\sqrt2$). Classical solution: $\vec x = A^{-1}\vec b = (0.75,\; {-0.25})^\top$
```csharp
import Std.Math.*;
import Std.Convert.*;
import Std.Measurement.*;
import Std.Canon.*;

// Toy implementation - conceptual sketch for 2×2 diagonal A = diag(1, 2) only.
// e^{iAt}: |0⟩ → e^{it}|0⟩ (eigenvalue λ₁=1), |1⟩ → e^{i2t}|1⟩ (eigenvalue λ₂=2)
// In QPE, the relevant quantity for phase kickback is the relative phase between
// eigenstates. |1⟩ accumulates e^{i·angle} more phase than |0⟩ per application.
// R1(angle) applies exactly this relative phase.
// Note: this encodes the phase *difference* (λ₂ - λ₁)·t, not both absolute phases.
// A production circuit encoding both eigenvalues' absolute phases requires an
// additional global phase gate; see Babbush et al. for resource-efficient constructions.
operation TimeEvolutionA(t : Double, power : Int, system : Qubit[]) : Unit is Adj + Ctl {
    let angle = t * IntAsDouble(power);
    R1(angle, system[0]); // relative phase e^{i·angle} on |1⟩ vs |0⟩
}

// Controlled rotation: ancilla ← C/λ where λ is encoded in clock
// For 2-bit clock: λ ∈ {1, 2} → rotate by arcsin(C/λ)
operation EigenvalueInversion(clock : Qubit[], ancilla : Qubit, C : Double) : Unit is Adj {
    let twoToT = 1 <<< Length(clock);
    for k in 0..twoToT - 1 {
        if k != 0 {
            let lambda = IntAsDouble(k);
            let angle = 2.0 * ArcSin(C / lambda);
            ControlledOnInt(k, Ry(angle, _))(clock, ancilla);
        }
    }
}

operation HHL(nClock : Int, t : Double, C : Double) : Result {
    use (clock, system, ancilla) = (Qubit[nClock], Qubit[1], Qubit());

    // Encode |b⟩ = |0⟩ (already default state)
    // Step 1+3: QPE & inverse QPE (within block auto-uncomputes)
    within {
        ApplyToEach(H, clock);
        for j in 0..nClock - 1 {
            Controlled TimeEvolutionA([clock[j]], (t, 1 <<< j, system));
        }
        Adjoint ApplyQFT(clock);
    }
    apply {
        // Step 2: eigenvalue inversion
        EigenvalueInversion(clock, ancilla, C);
    }
    // Post-select on ancilla = 1
    let res = MResetZ(ancilla);
    ResetAll(clock + system);
    return res;
    // If res == One: system now holds |x⟩ ∝ A⁻¹|b⟩
}
```
`within/apply` block runs [[QPE]] in `within`, eigenvalue inversion in `apply`, then auto-applies inverse [[QPE]] - this is exactly the uncomputation needed in [[HHL]].
### Limitations & fine print
- **Input model**: loading $|b\rangle$ from classical data costs $O(N)$ (QRAM required for true speedup)
- **Condition num $\kappa$**: circuit depth scales as $O(\kappa^2 \log N / \epsilon)$ - ill-conditioned matrices lose the speedup
- **Output**: you get $|x\rangle$, not the classical [[Vector]] - reading out all components destroys the quantum speedup
- **Practical use**: [[HHL]] provides speedup only when $A$ is sparse, $\kappa$ is small, & the answer can be obtained from $O(1)$ inner products rather than full readout
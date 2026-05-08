#Algorithm #Math
**[[HHL]] algorithm** (Harrow-Hassidim-Lloyd, $2009$) solves linear system $A\vec{x} = \vec{b}$ exponentially faster than classical Gaussian elimination for certain sparse, well-conditioned matrices. [[QPE]] is its core subroutine: it extracts the eigenvalues of $A$ so they can be inverted quantumly.
### Structure of HHL
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

$|b\rangle = |0\rangle = \frac{1}{\sqrt2}(|u_2\rangle + |u_1\rangle \cdot(-1)^?)$... Solution: $\vec x = A^{-1}\vec b = (0.75, -0.25)$
```csharp
import Std.Math.*;
import Std.Convert.*;
import Std.Measurement.*;
import Std.Canon.*;

// Time evolution for A = diag(1, 2) (already diagonal in Z basis)
// e^{iAt}: |0⟩ → e^{it}|0⟩, |1⟩ → e^{i2t}|1⟩
// Implemented as R1(t)|0⟩ & R1(2t)|1⟩ = phase kick via Rz
operation TimeEvolutionA(t : Double, power : Int, system : Qubit[]) : Unit is Adj + Ctl {
    let angle = t * IntAsDouble(power);
    // |0⟩ eigenvalue 1 → phase e^{iangle}
    // |1⟩ eigenvalue 2 → phase e^{i2angle}
    R1(angle,  system[0]); // e^{iλ₁·angle} on |0⟩
    R1(angle, system[0]); // extra phase for |1⟩: R1(2angle) = R1(angle)²
    Controlled R1([system[0]], (angle, system[0])); // conditional extra phase
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
### See also
- [[QPE]] - full [[QPE]] theory & circuit
- [[QPE apps]] - all [[QPE]] use cases
### Sources
- [Harrow, Hassidim & Lloyd 2009: Quantum algorithm for linear systems of equations](https://arxiv.org/abs/0811.3171)
- [Aaronson 2015: Read the fine print (HHL caveats)](https://www.scottaaronson.com/papers/qml.pdf)
- [Childs, Kothari & Somma 2017: Solving systems of linear equations with quantum mechanics](https://arxiv.org/abs/1512.01029)
- [Qiskit: HHL tutorial](https://learning.quantum.ibm.com/tutorial/solving-linear-systems-of-equations-using-hhl-and-its-qiskit-implementation)

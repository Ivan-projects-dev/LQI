#Algorithm #Math
**[[QPE]] in optimization** casts combinatorial & continuous optimization problems as Hamiltonian ground-state problems, then extracts the optimum exactly via [[QPE]]. Provides fault-tolerant, non-variational alternative to [[QAOA]] & [[VQE]].
### Ising / QUBO ground state
Many NP-hard problems map to **Ising Hamiltonian** (see [[QUBO]]):
$$H_C = \sum_{i<j} J_{ij} Z_i Z_j + \sum_i h_i Z_i$$
Eigenvalues of $H_C$ are objective func values $f(x)$; ground state $|\psi_0\rangle$ encodes optimal bitstring $x^*$

[[QPE]] on $U(\tau) = e^{-iH_C\tau}$ extracts ground-state energy $E_0 = f(x^*)$ to precision $\epsilon$ using $O(1/\epsilon)$ queries. Unlike [[QAOA]] which estimates $\langle H_C\rangle$ variationally, [[QPE]] extracts the exact [[Eigenvalue]].

**Caveat**: [[QPE]] outputs $E_0$ (optimal value), not $x^*$ (optimal solution) directly. To recover $x^*$: combine with amplitude amplification using energy filter (see [[Ground State Filtering]]) to boost probability of sampling $|\psi_0\rangle$, then measure in computational basis.

**Resource count**: $n$ [[Qubits]] for $n$-variable problem + $t$ clock [[Qubits]] for precision + [[Ancilla]] for $e^{-iH_C\tau}$. Implementing $e^{-iH_C\tau}$ for Ising: each $ZZ$ term is controlled phase gate - $O(m)$ $2$-qubit gates for $m$ couplings per Trotter step.
### Eigenvalue filtering for optimization
If lower bound $E_L \leq E_0$ is known (e.g., from SDP relaxation of the Ising problem), apply spectral filter centered at $E_L$ to amplify ground-state component before or during [[QPE]]. Reduces effective spectral search range from full spectrum width $\sim \sum|J_{ij}|$ to tighter window $E_0 - E_L$.

**SDP lower bounds + filtering workflow**:
1. Solve classical SDP relaxation $→ E_{\rm SDP} \leq E_0$
2. Apply Gaussian or power-cosine filter centered at $E_{\rm SDP}$
3. Run [[QPE]] on filtered state $→$ high success prob even with poor initial trial state
Effective when SDP bound is tight (e.g., Max-Cut: $E_{\rm SDP}/E_0 \geq 0.878$ by **Goemans-Williamson**).
### QPE-assisted QAOA
[[QAOA]] requires classical optimization over $(\gamma, \beta)$ parameters via noisy expectation value estimates. Core difficulty: **barren plateau** - gradient $\partial\langle H_C\rangle/\partial\gamma$ vanishes exponentially in $n$ for random params.

[[QPE]] assistance:
- **Exact energy [[Oracle]]**: run [[QPE]] on $|\psi(\gamma,\beta)\rangle = \prod_i e^{-i\gamma_i H_C}e^{-i\beta_i H_B}|+\rangle^n$ to get $\langle H_C\rangle(\gamma,\beta)$ exactly (no shot noise). Enables gradient-free optimizer with exact func evaluations
- **Spectral overlap estimation**: [[QPE]] on the [[QAOA]] state tells how much overlap it has with each energy [[Eigenvalue]] $→$ diagnose whether [[QAOA]] is converging or stuck at excited state
- **Depth $p$ selection**: estimate $\langle H_C\rangle$ exactly as func of $p$ using [[QPE]] - identify min $p$ where [[QAOA]] output has sufficient overlap with $|\psi_0\rangle$
Circuit depth of [[QAOA]] unitary: $O(p\cdot m)$ $2$-qubit gates for $m$ coupling terms; much shallower than full [[Hamiltonian simulation]], making [[QPE]] on it feasible at moderate precision.
### Quantum semidefinite programming
**SDP**: minimize $\text{tr}(CX)$ subject to $X\succeq 0$, $\text{tr}(A_i X) = b_i$ for $i=1,\ldots,m$.

Classical interior point: $O(m^{0.5}n^{2.37}\log(1/\epsilon))$ per iteration. Quantum speedup via [[QPE]]:
- [[Eigenvalue]] access to $X$ via [[QPE]] on $e^{iXt}$ enables $O(\text{polylog}(n))$ [[Matrix]]-[[Vector]] products
- [[HHL]] for linear system solve in each interior-point step: $O(\kappa^2\log n/\epsilon)$
- Combined quantum SDP: $O(\sqrt{m}\cdot\kappa^2\log n/\epsilon)$ vs classical $O(\sqrt{m}\cdot n^{2.37})$

**Limitation**: genuine quantum advantage requires QRAM for data access & small condition num $\kappa$. Dequantization results (Tang et al.) show that sampling access to classical data can sometimes match quantum speedup - advantage regime requires structured, high-rank inputs.
### Quantum interior point methods for LP/QP
Linear programs (LP): minimize $c^Tx$ s.t. $Ax = b$, $x\geq0$. Each interior-point iteration solves normal equations $(AD^2A^T)\Delta y = r$ where $D$ = diagonal scaling. Quantum speedup via [[HHL]] (which uses [[QPE]] internally):
- Classical: $O(m^{0.5}n^{1.5})$ per iteration (best known)
- Quantum: $O(\sqrt{m}\cdot n\cdot\kappa^2/\epsilon)$ - advantage when $m \gg n$ & $\kappa$ small

Quadratic programs (QP): same structure with extra quadratic term in Hessian; quantum speedup carries over if Hessian has efficiently accessible block structure.
### Comparison to variational methods

| Method           | Accuracy                      | Hardware | Handles excited states? | Outputs solution?  |
| ---------------- | ----------------------------- | -------- | ----------------------- | ------------------ |
| [[QAOA]]         | Heuristic (approx ratio)      | NISQ     | No                      | Yes (measurement)  |
| [[VQE]]          | Variational upper bound       | NISQ     | Partially (VQD)         | No (energy only)   |
| [[QPE]] (Ising)  | Exact $E_0$ to $\epsilon$     | [[FTQC]] | Yes                     | With amplification |
| [[QPE]] + filter | Exact $E_0$, improved overlap | [[FTQC]] | Yes                     | Yes                |


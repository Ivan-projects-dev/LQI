#Algorithm #Chemistry #Physics
**State preparation** is the task of creating a [[Quantum state]] $|\psi\rangle$ that has sufficient overlap with the target [[Eigenstate]] (e.g., ground state $|E_0\rangle$) before running [[QPE]]. It is often the **hardest** step in quantum chemistry simulation.

### Why it matters
[[QPE]] returns [[Eigenphase]] $\varphi_k$ with probability $|c_k|^2$ where $|\psi\rangle = \sum_k c_k |E_k\rangle$. Ground state success probability:
$$P(\text{measure } E_0) = |\langle E_0 | \psi\rangle|^2 = \eta$$

If $\eta$ is small, [[QPE]] must be repeated $O(1/\eta)$ times - directly multiplying the total gate count. Typical targets: $\eta \geq 0.1$ (10% overlap) to keep overhead manageable.

**Hartree-Fock (HF) state** - default starting point
Occupies the $M$ lowest-energy spin-orbitals (single Slater determinant). Maps to a simple product state on [[Qubits]] - just apply $X$ gates to occupied orbitals:
$$|\text{HF}\rangle = |1,1,\ldots,1,0,0,\ldots,0\rangle$$
**Circuit cost**: $O(N)$ $X$ gates. **Overlap**: $\eta \sim 0.5$–$0.99$ for weakly correlated molecules (closed-shell); can be $\eta < 0.01$ for strongly correlated systems (e.g., transition metals, bond-breaking).

**Multi-reference / CASSCF state**
Uses a classical CASSCF calculation to identify dominant configurations, then prepares a superposition:
$$|\psi_{\text{MREF}}\rangle = \sum_{i=1}^{K} c_i |\text{det}_i\rangle$$
**Circuit cost**: $O(K \cdot N)$ with sparse state preparation. **Overlap**: much better than HF for strongly correlated systems.

**VQE ansatz as initial state**
Run [[VQE]] to convergence, then use the optimized VQE state as QPE input. VQE cannot match QPE accuracy, but provides a good $|\psi_0\rangle$ approximation.
**Overlap**: depends on ansatz quality; UCCSD gives $\eta > 0.9$ for small molecules.

**QROM-based state preparation**
Quantum Read-Only Memory (QROM) encodes a classically precomputed state (e.g., FCI coefficients for small active space) directly into quantum amplitudes. Requires $O(K)$ T-gates for $K$-term superposition.
**Circuit cost**: $O(K \log K / \log\log K)$ T-gates. Used in fault-tolerant resource estimates.

**Adiabatic state preparation**
Slowly evolve from the ground state of a simple Hamiltonian $H_B$ (e.g., transverse field) to the target $H_P$ via the [[Adiabatic Quantum Optimization]] schedule. Theoretically correct but circuit depth scales as $O(\Delta_{\min}^{-2})$ - impractical when gap is small.

### Overlap amplification (filtering)
When $\eta$ is small, [[Ground State Filtering]] techniques amplify the overlap before or during [[QPE]]:
- **Gaussian filter**: $F = e^{-(H-E_*)^2/2\sigma^2}$, requires rough energy estimate $E_*$
- **QSP power-cosine filter**: $O(\Delta^{-1}\log(\eta^{-1}))$ queries, near-optimal
- **Krylov**: $d$ Hamiltonian applications + classical diagonalization

| Method          | Circuit cost                 | Overlap $\eta$ | Works for                  |
| --------------- | ---------------------------- | -------------- | -------------------------- |
| Hartree-Fock    | $O(N)$                       | $0.5–0.99$     | Weakly correlated          |
| Multi-reference | $O(KN)$                      | $0.5-0.95$     | Moderately correlated      |
| [[VQE]] ansatz      | Variational                  | $0.8-0.99$     | NISQ-accessible sizes      |
| QROM            | $O(K\log K)$ T-gates         | $~1.0$         | Any (if classically known) |
| Adiabatic       | $O(\Delta_{\min}^{-2})$      | $1.0$ (ideal)  | Gapped systems             |
| + Filtering     | $O(\Delta^{-1}/\sqrt{\eta})$ | Amplified      | Any $\eta > 0$             |
### QPE connection
Total [[QPE]] cost (gates) scales as $O(\text{cost}(U^{2^t}) / \eta)$ where $\eta$ is the overlap. A $10\times$ improvement in state preparation can reduce total circuit count by $10\times$ - often more impactful than optimizing [[QPE]] itself.

See [[QPE inputs]] for the Q# perspective. 
See [[Ground State Filtering]] for overlap amplification. 
See [[Second quantization]] for how molecular states are encoded.

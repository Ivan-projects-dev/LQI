#Math #Physics
**Hermitian operator** $A$ satisfies $A = A^\dagger$ (equals its own conjugate transpose). This single condition has far-reaching physical consequences.

$$A^\dagger = (A^*)^T = A$$

### Why Hermiticity matters in quantum mechanics
Every physically measurable quantity (observable) is represented by a Hermitian operator. This is required because:
- **Real eigenvalues** - measurement outcomes must be real numbers (see [[Eigenvalue]])
- **Orthogonal eigenstates** - distinct energy levels have orthogonal states; measurements are mutually exclusive
- **Complete basis** - eigenstates span the full Hilbert space, so any state can be expanded in them

### Key properties

| Property | Statement |
|----------|-----------|
| Real spectrum | All [[Eigenvalue]]s $\lambda_k \in \mathbb{R}$ |
| Orthogonal eigenstates | $\langle u_j \vert u_k \rangle = \delta_{jk}$ for distinct $\lambda_j \neq \lambda_k$ |
| Diagonalizable | $A = U D U^\dagger$, $D$ real diagonal, $U$ unitary |
| Generates unitary | $e^{iAt}$ is unitary for any real $t$ - used in time evolution |

### Proof: eigenvalues are real
For eigenstate $|u\rangle$ with $A|u\rangle = \lambda|u\rangle$:
$$\langle u|A|u\rangle = \lambda \qquad \langle u|A^\dagger|u\rangle = \lambda^*$$
Since $A = A^\dagger$: $\lambda = \lambda^*$, so $\lambda \in \mathbb{R}$.

### Hermitian vs unitary
Hermitian and unitary operators play complementary roles:

| | Hermitian $H = H^\dagger$ | Unitary $U^\dagger U = I$ |
|-|--------------------------|--------------------------|
| Eigenvalues | Real $\lambda \in \mathbb{R}$ | Unit circle $\vert\lambda\vert = 1$ |
| Role | Observable / [[Hamiltoan\|Hamiltonian]] | Time evolution / gate |
| Relationship | $U = e^{iHt}$ converts one to the other | |
| QPE target | $H$ encodes energy levels | $U = e^{-iHt}$ is what [[QPE]] runs |

### Common Hermitian operators in quantum computing

| Operator | Hermitian? | Eigenvalues |
|----------|-----------|-------------|
| Pauli $X, Y, Z$ | ✓ | $\pm 1$ |
| [[Hadamard]] $H$ | ✓ | $\pm 1$ |
| $S, T$ gates | ✗ (unitary only) | on unit circle |
| [[Hamiltoan\|Hamiltonian]] $H$ | ✓ (required) | energy levels $E_k$ |
| Projection $\Pi = \vert\psi\rangle\langle\psi\vert$ | ✓ | $0$ or $1$ |

### Anti-Hermitian
$A = -A^\dagger$ has purely imaginary eigenvalues. $iH$ is anti-Hermitian when $H$ is Hermitian - this is why the [[Schrödinger equation]] has the factor $i$: $\frac{d}{dt}|\psi\rangle = -iH|\psi\rangle$ keeps norm conserved.

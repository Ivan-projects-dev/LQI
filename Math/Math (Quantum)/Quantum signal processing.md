#Algorithm #Math
**Quantum Signal Processing (QSP)** is a framework for implementing polynomial transformations of operator eigenvalues on a quantum computer. It underlies the most query-optimal quantum algorithms known - including [[Hamiltonian simulation]], [[Ground State Filtering]], and amplitude estimation.

### Core idea
A QSP circuit alternates a fixed "signal" unitary $W(x)$ (encoding a scalar $x \in [-1,1]$) with single-qubit "processing" rotations $R(\phi_k) = e^{i\phi_k Z}$:
$$U_{\Phi} = R(\phi_0)\prod_{k=1}^{d} W(x)\, R(\phi_k)$$

By choosing the phase angles $\Phi = (\phi_0,\ldots,\phi_d)$ appropriately, the top-left entry of $U_\Phi$ becomes any degree-$d$ polynomial $p(x)$ satisfying:
- $|p(x)| \leq 1$ for all $x \in [-1,1]$
- Definite parity: $p(-x) = \pm p(x)$

This is the **QSP theorem**: the reachable polynomials are exactly the bounded, definite-parity ones.

### Signal operator
For a [[Block encoding]] of Hamiltonian $H/\lambda$ (so eigenvalues $E_k/\lambda \in [-1,1]$), the signal operator is the block-encoded unitary itself:
$$W = \begin{bmatrix} H/\lambda & \cdot \\ \cdot & \cdot \end{bmatrix}$$

The QSP circuit then implements $p(H/\lambda)$ in the same block - applying the polynomial transformation to **all eigenvalues simultaneously**.

### Quantum Singular Value Transformation (QSVT)
QSVT is the generalization of QSP to non-Hermitian (non-square) matrices via singular value decomposition. For a block-encoded [[Matrix]] $A$, QSVT implements polynomial $p(\sigma)$ on all singular values $\sigma$ of $A$. QSVT unifies nearly every known quantum speedup into a single framework.

### Applications via polynomial selection

| Target operation | Polynomial $p(x)$ | Degree | Algorithm |
|-----------------|-------------------|--------|-----------|
| $e^{-iHt}$ (time evolution) | $e^{-i\lambda x t}$ (Jacobi-Anger) | $O(\lambda t + \log(1/\epsilon))$ | [[Hamiltonian simulation]] |
| $\text{sgn}(x)$ (ground state filter) | Chebyshev approx to sign | $O(\Delta^{-1}\log(\eta^{-1}))$ | [[Ground State Filtering]] |
| $1/x$ ([[Matrix]] inversion) | Polynomial approx to $1/x$ | $O(\kappa\log(1/\epsilon))$ | [[HHL]] |
| $\sqrt{1-x^2}$ (amplitude amp.) | Chebyshev | $O(1/\sqrt{\epsilon})$ | [[Grover]]-like search |
| Threshold $[x>\theta]$ | Smooth step function | $O(\Delta^{-1})$ | Spectral filtering |

### Phase finding (connection to QPE)
When $p(x) = e^{i\arccos(x)}$ (the [[Eigenphase]]), QSP implements the same transformation as [[QPE]] but with $O(1/\epsilon)$ queries and no QFT - achieving Heisenberg-limited scaling. This is the basis of the Dong-Lin-Tong (2022) single-[[Ancilla]] [[QPE]] algorithm referenced in [[Early FTQC]].

### Finding phase angles
Given target polynomial $p(x)$, finding angles $\Phi$ is non-trivial. Methods:
- **Optimization**: minimize $\|U_\Phi - p\|$ via gradient descent (works for $d \lesssim 100$)
- **Prony / GSLW method**: exact algebraic solution, numerically unstable for large $d$
- **Chebyshev basis**: `pyqsp` library uses Chebyshev representation, more numerically stable
- **Adiabatic-impulse model** (2025): efficient approximate construction for large $d$

### Resource cost
For degree-$d$ polynomial with [[Block encoding]] cost $C$:
- Gates: $O(d \cdot C)$
- [[Ancilla]]: same as [[Block encoding]] ($O(\log N)$)
- Queries to $W$: exactly $d$

This is **optimal** - any quantum algorithm computing $p(H)$ requires $\Omega(d)$ queries.

### PennyLane
```python
# QSVT applies polynomial to singular values of block-encoded matrix
qml.QSVT(block_encoded_op, projectors, phase_angles)
```
See `[[PennyLane]].ai/qml/demos/tutorial_intro_qsvt` for worked examples.

Source: [Martyn et al. — "Grand Unification of Quantum Algorithms" (2021)](https://arxiv.org/abs/2105.02859) [pyqsp — GitHub](https://github.com/ichuang/pyqsp)

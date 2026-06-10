#Physics #Math
**Hamiltonian** $H$ is the [[Hermitian operator]] representing the total energy of a quantum system. Its [[Eigenvalue]]s are the allowed energy levels; its [[Eigenstate]]s are states with definite energy.

$$H|E_k\rangle = E_k|E_k\rangle \qquad E_k \in \mathbb{R}$$

[[Hermitian operator\|Hermiticity]] $H = H^\dagger$ guarantees real eigenvalues - required for physically measurable energies. See [[Eigenvalue]] (Hermitian proof).

### Time evolution
The Hamiltonian generates time evolution via the **[[Schrödinger equation]]**:
$$i\hbar\frac{d}{dt}|\psi(t)\rangle = H|\psi(t)\rangle$$
For time-independent $H$, the solution is the [[Unitary operator]]:
$$|\psi(t)\rangle = e^{-iHt/\hbar}|\psi(0)\rangle = U(t)|\psi(0)\rangle$$
(natural units $\hbar=1$ used throughout quantum computing literature)

### Spectrum
| Term | Definition                                                   |
|------|-----------|
| Ground state $\vert E_0\rangle$ | Eigenstate with lowest eigenvalue $E_0$                      |
| Excited states | $\vert E_1\rangle, \vert E_2\rangle,\ldots$ with $E_1 > E_0$ |
| Spectral gap | $\Delta = E_1 - E_0$ - controls adiabatic runtime & mixing   |
| Spectrum | Full set $\{E_0, E_1,\ldots\}$ = allowed energy levels       |

### Common Hamiltonians in quantum computing

| Hamiltonian | Form | Used in |
|-------------|------|---------|
| Pauli $Z$ | $H = Z$ | single-qubit phase evolution |
| Ising | $H = -\sum_{ij} J_{ij} Z_i Z_j$ | [[Quantum annealing]], [[QAOA]] |
| Transverse-field Ising | $H = -J\sum Z_i Z_{i+1} - h\sum X_i$ | [[Adiabatic Quantum Optimization]] driver |
| Molecular (2nd quantization) | $H = \sum_{pq} h_{pq} a_p^\dagger a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs} a_p^\dagger a_q^\dagger a_r a_s$ | [[QPE Chemistry]], [[VQE]] |
| Problem Hamiltonian $H_P$ | diagonal in $Z$ basis, encodes cost | [[QUBO Constraints]], [[Adiabatic Quantum Optimization]] |
| Mixer / driver $H_B$ | $-\sum_i X_i$ | [[QAOA]], [[Adiabatic Quantum Optimization]] |

### Time-independent vs time-dependent

**Time-independent** ($H$ constant): energy eigenstates evolve by pure phase $e^{-iE_k t}$, population unchanged. Used in [[QPE]], [[VQE]].

**Time-dependent** ($H = H(t)$): populations can change. Governs [[Adiabatic Quantum Optimization]] where $H(t) = (1-s)H_B + s H_P$.

### QPE connection
[[QPE]] extracts [[Eigenphase]] $\varphi_k = -E_k\tau/(2\pi)$ of $U(\tau) = e^{-iH\tau}$, giving energy $E_k = -2\pi\varphi_k/\tau$. The ground state energy $E_0$ is the primary target in quantum chemistry and optimization. See [[Eigenphase]], [[QPE Chemistry]].

### Diagonalization
Any Hermitian $H$ can be diagonalized: $H = U D U^\dagger$ where $D = \mathrm{diag}(E_0, E_1,\ldots)$ and $U$'s columns are the eigenstates. Then $e^{-iHt} = U\,e^{-iDt}\,U^\dagger$. See [[Eigenvector]] (diagonalization).

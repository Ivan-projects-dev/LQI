#Math #Physics
**Eigenvalue** $\lambda$ is the scalar that appears when operator $A$ acts on its [[Eigenstate]] (or [[Eigenvector]]):
$A|u\rangle = \lambda|u\rangle$ It encodes the physical quantity that measurement returns with certainty when the system is in $|u\rangle$.
### What eigenvalues encode by operator type

| Operator type                  | Eigenvalue $\lambda$                     | Physical meaning                                        |
| ------------------------------ | ---------------------------------------- | ------------------------------------------------------- |
| [[Hamiltoan\|Hamiltonian]] $H$ | Real num $E_k$                           | Energy level                                            |
| [[Unitary operator]] $U$       | Complex num on unit circle $e^{i\theta}$ | Phase accumulated per application                       |
| Pauli $Z$                      | $+1$ or $-1$                             | Measurement outcome ($0$ or $1$ in computational basis) |
| Pauli $X$                      | $+1$ or $-1$                             | Measurement outcome in $X$ basis                        |
| Projection $\Pi$               | $0$ or $1$                               | "Is system in subspace?"                                |
### Computing eigenvalues
Eigenvalues of $A$ are roots of the **characteristic polynomial**:
$$\det(A - \lambda I) = 0$$
For $2\times2$: $\lambda^2 - \mathrm{tr}(A)\,\lambda + \det(A) = 0$.
### Eigenvalues of Hermitian operators are always real
$H = H^\dagger \Rightarrow \langle u|H|u\rangle = \langle u|H^\dagger|u\rangle^* = \langle u|H|u\rangle^*$, so $\langle H\rangle \in \mathbb{R}$.
This guarantees that energy levels & measurement outcomes are real nums - a physical requirement.
### Eigenvalues of unitary operators lie on the unit circle
$U^\dagger U = I \Rightarrow |Uv|^2 = |v|^2$, so $|\lambda|^2 = 1 \Rightarrow \lambda = e^{i\theta}$.
Angle $\theta$ is the [[Eigenphase]]; it is what [[QPE]] extracts.
### Spectrum
Full set of eigenvalues $\{\lambda_0, \lambda_1, \ldots\}$ of operator is its **spectrum**. For [[Hamiltoan|Hamiltonian]], spectrum = allowed energy levels of the physical system. Smallest eigenvalue is the ground-state energy $E_0$.

**Spectral gap** = $E_1 - E_0$ (gap between ground & first excited state). Determines difficulty of ground-[[State preparation]] & speed of quantum walks. See [[QPE Walks]].
#Math
**Eigenvector** of square [[Matrix]] $A$ is non-$0$ [[Vector]] $v$ that $A$ maps to scalar multiple of itself: $Av = \lambda v$
$\lambda$ is [[Eigenvalue]]. [[Vector]]'s direction is preserved; only its magnitude (& sign/phase) changes.

**Finding eigenvectors**: solve $(A - \lambda I)v = 0$. Non-trivial solution exists iff $\det(A - \lambda I) = 0$ - **characteristic equation** that yields the eigenvalues first.
### Example - $2\times 2$ rotation-like matrix
$$A = \begin{pmatrix}0 & 1 \\ 1 & 0\end{pmatrix} \quad \text{(Pauli X / S in 2D)}$$
Where $S$ - [[SWAP]] in $2D$
Characteristic equation: $\det\begin{pmatrix}-\lambda & 1 \\ 1 & -\lambda\end{pmatrix} = \lambda^2 - 1 = 0 \Rightarrow \lambda = \pm 1$
Eigenvectors:
- $\lambda = +1$: $(A - I)v = 0 \Rightarrow v = (1,\,1)^T/\sqrt{2}$
- $\lambda = -1$: $(A + I)v = 0 \Rightarrow v = (1,\,-1)^T/\sqrt{2}$
### Eigenvector basis (diagonalization)
If $A$ has $n$ linearly independent eigenvectors $v_1,\ldots,v_n$ with eigenvalues $\lambda_1,\ldots,\lambda_n$:
$$A = P\,\Lambda\,P^{-1} \qquad \Lambda = \mathrm{diag}(\lambda_1,\ldots,\lambda_n)$$
$P$ = [[Matrix]] whose columns are the eigenvectors. Computing $A^k = P\Lambda^k P^{-1}$ is then trivial - used to implement $U^{2^j}$ efficiently in [[QPE]].
### Hermitian & unitary matrices
- **Hermitian** $A = A^\dagger$: all eigenvalues are **real**. Eigenvectors for distinct eigenvalues are orthogonal. This is why Hamiltonians (always Hermitian) have real energy levels.
- **Unitary** $U^\dagger U = I$: all eigenvalues have magnitude $1$, i.e. $\lambda = e^{i\theta}$. Eigenvectors form orthonormal basis. Angle $\theta$ is the [[Eigenphase]].
Quantum-mechanical counterpart: [[Eigenstate]]. Same object, different notation.
### Eigenvectors of common quantum gates

| [[Matrix]] $A$ | [[Eigenvalue]] $\lambda$ | Eigenvector $v$ | [[Eigenstate]] |
|------------|--------------------------|-----------------|----------------|
| Pauli $Z$ | $+1$ | $(1,\,0)^T$ | $\vert0\rangle$ |
| Pauli $Z$ | $-1$ | $(0,\,1)^T$ | $\vert1\rangle$ |
| Pauli $X$ | $+1$ | $(1,\,1)^T/\sqrt{2}$ | $\vert{+}\rangle$ |
| Pauli $X$ | $-1$ | $(1,\,-1)^T/\sqrt{2}$ | $\vert{-}\rangle$ |
| Pauli $Y$ | $+1$ | $(1,\,i)^T/\sqrt{2}$ | $\vert{+i}\rangle$ |
| Pauli $Y$ | $-1$ | $(1,\,-i)^T/\sqrt{2}$ | $\vert{-i}\rangle$ |
| [[Hadamard]] $H$ | $+1$ | $(\cos\frac{\pi}{8},\,\sin\frac{\pi}{8})^T$ | not a Pauli basis state |
| [[Hadamard]] $H$ | $-1$ | $(-\sin\frac{\pi}{8},\,\cos\frac{\pi}{8})^T$ | |
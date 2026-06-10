#Math #Physics #Algorithm
**Eigenphase** $\varphi$ is angle in [[Eigenvalue]] of [[Unitary operator]]:
$$U|u\rangle = e^{2\pi i\varphi}|u\rangle \qquad \varphi \in [0,\, 1)$$
Since unitary eigenvalues always lie on the unit circle ($|\lambda|=1$), the full [[Eigenvalue]] is captured by $\varphi$ alone. [[QPE]] is the algorithm that extracts $\varphi$ to $t$ bits of precision.

| Convention | [[Eigenvalue]] form | Phase range | Used in |
|-----------|----------------|-------------|---------|
| $e^{2\pi i\varphi}$ | $\varphi \in [0,1)$ | fractional binary | [[QPE]] (standard) |
| $e^{i\theta}$ | $\theta \in [0, 2\pi)$ | radians | physics, spectroscopy |

Conversion: $\theta = 2\pi\varphi$.
### Eigenphase from physical Hamiltonian
Time evolution $U(\tau) = e^{-iH\tau}$ gives eigenphase:
$$e^{-iE_k\tau} = e^{2\pi i\varphi_k} \quad\Rightarrow\quad \varphi_k = \frac{-E_k\tau}{2\pi} \pmod{1}$$
Inverting: $E_k = -2\pi\varphi_k/\tau$. This is the post-processing step in all [[QPE Chemistry]] & [[QPE Simulation]] applications.

**Sign & wrap-around**: negative energies give positive $\varphi$ (since $\varphi = -E\tau/2\pi$ & $E < 0$). Eigenphases $> 0.5$ correspond to negative energies if $\tau > 0$. When reading QPE output bins $> N/2$, subtract $1$ to get the negative-phase branch: $\varphi_\text{signed} = \varphi - 1$ if $\varphi > 0.5$.

### Eigenphases of standard gates

| Gate $U$ | [[Eigenstate]] | Eigenphase $\varphi$ | Binary fraction |
|----------|---------------|----------------------|----------------|
| $Z$ | $\vert0\rangle$ | $0$ | $0.000\ldots$ |
| $Z$ | $\vert1\rangle$ | $1/2$ | $0.100\ldots$ |
| $S$ | $\vert1\rangle$ | $1/4$ | $0.010\ldots$ |
| $T$ | $\vert1\rangle$ | $1/8$ | $0.001\ldots$ |
| $R_k$ | $\vert1\rangle$ | $1/2^k$ | $k$-th bit set |

The $T$ gate has eigenphase $1/8$, requiring exactly $3$ clock [[Qubits]] to resolve perfectly - this is the standard [[QPE on T gate]] example.

### QPE precision
With $t$ clock [[Qubits]], [[QPE]] resolves $\varphi$ to the nearest multiple of $1/2^t$. The estimate $\tilde\varphi$ satisfies:
$$|\tilde\varphi - \varphi| \leq \frac{1}{2^{t+1}}$$
with probability $\geq 8/\pi^2 \approx 0.81$ using exactly $t$ bits, or $\geq 1 - \epsilon$ with $t + \lceil\log_2(1/2\epsilon)\rceil$ bits. See [[QPE precision]].

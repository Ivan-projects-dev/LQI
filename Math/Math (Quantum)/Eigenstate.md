#Math #Physics 
**Eigenstate** of operator $A$ is [[Quantum state]] $|u\rangle$ that $A$ maps to itself scaled by scalar:
$$A|u\rangle = \lambda|u\rangle$$
The state direction is preserved - only scalar factor $\lambda$ (the [[Eigenvalue]]) is picked up. Quantum-mechanical counterpart of [[Eigenvector]]; distinction is purely notational (bra-ket vs column [[Vector]]).

**Measurement consequence**: measuring observable $A$ on its eigenstate $|u\rangle$ always returns $\lambda$ with certainty - no probabilistic spread.

**Non-eigenstate input**: if $|\psi\rangle = \sum_k c_k |u_k\rangle$, measuring $A$ returns $\lambda_k$ with probability $|c_k|^2$ & collapses to $|u_k\rangle$. In [[QPE]] this means the clock register spreads across bins weighted by $|c_k|^2$; success probability for ground state = $|\langle E_0|\psi_\text{trial}\rangle|^2$. See [[QPE inputs]]

| Gate | Eigenstate | [[Eigenvalue]] $\lambda$ | [[Eigenphase]] $\varphi$ |
|------|-----------|--------------------------|--------------------------|
| $Z$ | $|0\rangle$ | $+1$ | $0$ |
| $Z$ | $|1\rangle$ | $-1$ | $1/2$ |
| $S$ | $|0\rangle$ | $+1$ | $0$ |
| $S$ | $|1\rangle$ | $i$ | $1/4$ |
| $T$ | $|0\rangle$ | $+1$ | $0$ |
| $T$ | $|1\rangle$ | $e^{i\pi/4}$ | $1/8$ |
| $X$ | $|{+}\rangle$ | $+1$ | $0$ |
| $X$ | $|{-}\rangle$ | $-1$ | $1/2$ |

### Energy eigenstates (Hamiltonian)
For [[Hamiltoan\|Hamiltonian]] $H$: $H|E_k\rangle = E_k|E_k\rangle$. Lowest-energy eigenstate $|E_0\rangle$ is the **ground state** - target of [[QPE Chemistry]].
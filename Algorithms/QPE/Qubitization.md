#Algorithm #Math #Chemistry
**Qubitization** constructs unitary $W$ (the **walk operator**) whose eigenphases directly encode eigenvalues of $H$ - eliminating Trotter error entirely. Contrast with [[Trotter formula|Trotterization]], which approximates $e^{-iHt}$ with systematic error $O(\Delta t^p)$; qubitization is exact up to [[QPE]] approximation.
### From Hamiltonian to walk operator
**Step $1$ - LCU decomposition**: express $H$ as **linear combination of unitaries (LCU)**:
$$H = \sum_{\ell=1}^{L} \alpha_\ell U_\ell, \quad \alpha_\ell > 0, \; U_\ell \text{ unitary}$$
For molecular Hamiltonians: $U_\ell$ = Pauli strings, $\alpha_\ell$ = real coefficients. Define **1-norm** $\lambda = \sum_\ell \alpha_\ell$.
**Step $2$ - PREP & SEL oracles**:
- $\text{PREP}|0\rangle = \sum_\ell \sqrt{\alpha_\ell/\lambda}\,|\ell\rangle$ - prepares weighted superposition over LCU terms
- $\text{SEL} = \sum_\ell |\ell\rangle\!\langle\ell| \otimes U_\ell$ - applies $U_\ell$ to system register, controlled on index $|\ell\rangle$
**Step $3$ - walk operator**:
$$W = \bigl(2\,\text{PREP}\,\text{PREP}^\dagger - I\bigr)\cdot\text{SEL}$$
$W$ is Szegedy-type walk on extended space $\mathcal{H}_{\text{anc}}\otimes\mathcal{H}_{\text{sys}}$.
**Eigenphase relation**: if $H|E_j\rangle = E_j|E_j\rangle$, then $W$ has eigenvalues $e^{\pm i\arccos(E_j/\lambda)}$. Running [[QPE]] on $W$ $Q(W)$:
$$\text{Q}(W) \;\longrightarrow\; \tilde\varphi_j \approx \frac{\arccos(E_j/\lambda)}{2\pi}$$
$$E_j = \lambda\cos(2\pi\tilde\varphi_j)$$

| Method              | Circuit depth                                    | Error scaling            | Trotter error |
| ------------------- | ------------------------------------------------ | ------------------------ | ------------- |
| 1st-order Trotter   | $O\!\left(\lambda^2/\epsilon\right)$             | $O(\Delta t)$ per step   | Yes           |
| $p$th-order Trotter | $O\!\left(\lambda^{1+1/p}/\epsilon^{1/p}\right)$ | $O(\Delta t^p)$ per step | Yes           |
| Qubitization        | $O\!\left(\lambda/\epsilon\right)$               | Exact eigenphases        | None          |
For electronic structure: $\lambda \sim O(N^2)$ in $2nd$ quantization (Pauli basis), or $O(N)$ for structured sparse/factored basis sets. Qubitization achieves **linear scaling** in $\lambda/\epsilon$ - no multiplicative Trotter overhead.
### PREP oracle construction
$\text{PREP}$ needs $\lceil\log_2 L\rceil$ [[Ancilla]] [[Qubits]] for the LCU index register. Efficient circuits:
- **QROM** (quantum read-only memory): loads $\sqrt{\alpha_\ell}$ values from classical table - $O(L)$ T-gates
- **Alias sampling**: prepares $\text{PREP}$ using $O(L)$ one- & two-qubit gates with $O(\log L)$ [[Ancilla]] [[Qubits]]
- **Dicke state prep** $(2025)$: Fast One-Qubit-Controlled Select LCU (FOQCS-LCU) - linear [[Ancilla]] count, explicit decomposition into $1$- & $2$-qubit gates
### Hamiltonian factorization
Raw Pauli decomposition gives $L = O(N^4)$ terms. Tensor factorization reduces this:
- **Single factorization (SF)**: $L = O(N^2)$ - decomposes **electron repulsion integrals (ERI)** into rank-$1$ products
- **Double factorization (DF)**: $L = O(N)$ - further diagonalizes each SF factor; dominant approach for chemistry
- **Tensor hypercontraction (THC)**: $L = O(N)$ with smaller prefactor than DF; requires $O(N)$ non-Clifford gates per PREP step
### Resource estimates for chemistry
Using qubitization with DF of ERI (chemical accuracy $\epsilon = 1.6\times10^{-3}$ hartree):

| System | Spin orbitals | $\lambda$ (hartree) | Logical [[Qubits]] | T-gates |
|---|---|---|---|---|
| FeMoco (nitrogen fixation) | $\sim 54$ | $\sim 1800$ | $\sim 1000$ | $\sim 10^{10}$ |
| Cytochrome P450 | $\sim 58$ | $\sim 2600$ | $\sim 1200$ | $\sim 4\times10^{10}$ |
| Solid-state catalyst | $\sim 152$ | $\sim 10^4$ | $\sim 2400$ | $\sim 10^{11}$ |
T-gate counts roughly $100\times$ lower than Trotter-based [[QPE Chemistry]] at same $\epsilon$.
### Basis set optimization (2025)
[[QPE]] runtime scales as $O(\lambda/\epsilon)$; reducing $\lambda$ via basis set choice directly cuts cost. **Frozen natural orbital (FNO)** strategy: identify & freeze weakly-occupied orbitals, reducing $N$ & $\lambda$ significantly without sacrificing accuracy. Up to $10\times$ runtime reduction reported for mid-size molecules ($2025$ JCTC).
### See also
- [[QPE Chemistry]] - Trotter-based approach & overview
- [[Ground State Filtering]] - overlap amplification for poor trial states
- [[QPE apps]] - all [[QPE]] use cases
- [[Trotter formula]] - approximation replaced by qubitization
- [[QPE Walks]] - Szegedy walk operator (same construction)
### Sources
- [Babbush et al. 2019: Qubitization of Arbitrary Basis Quantum Chemistry](https://arxiv.org/abs/1902.02134)
- [Benchmarking Quantum Simulation Methods (Oct 2025)](https://arxiv.org/abs/2510.01710)
- [Improving QPE Runtime via Basis Set Optimization (JCTC 2025)](https://pubs.acs.org/doi/10.1021/acs.jctc.5c01512)
- [Efficient LCU block encodings through Dicke states (Jul 2025)](https://arxiv.org/abs/2507.20887)
- [Quantum Simulations of Chemistry in First Quantization (2024)](https://arxiv.org/abs/2408.03145)

#Algorithm #Math
**MNRS framework** (Magniez-Nayak-Roland-Santha, $2007$) is unified quantum walk search algorithm using [[QPE]] to implement **approximate reflection** in the walk eigenspace, enabling amplitude amplification over any quantizable Markov chain. Strictly generalizes [[Grover]] search to walks on arbitrary graphs.
### Setup
**Quantizable Markov chain**: classical random walk on $N$-node graph with transition [[Matrix]] $P$ & stationary distribution $\pi$. Quantum walk state: $|x\rangle|p_x\rangle$ where $|p_x\rangle = \sum_y\sqrt{P_{xy}}|y\rangle$.

MNRS requires $3$ quantum oracles:
- $\mathcal{S}$ (**Setup**): prepare $\sum_x\sqrt{\pi_x}|x\rangle|p_x\rangle|0\rangle_{\rm data}$. Cost: $S$.
- $\mathcal{C}$ (**Check**): given $|x\rangle$, decide if $x$ is marked (solution). Cost: $C$ per call.
- $\mathcal{U}$ (**Update**): given $|x\rangle|p_x\rangle$, perform one walk step $→$ $|x'\rangle|p_{x'}\rangle$ & update data structure. Cost: $U$.
**Spectral gap** $\delta$: gap between [[Eigenvalue]] $1$ & next [[Eigenvalue]] of walk operator $W$ applied to the **unmarked** subspace. Controls how fast the walk mixes.

**Marked fraction** $\epsilon = M/N$: fraction of "solution" nodes.

### QPE-based reflection
Core of MNRS: use [[QPE]] on walk operator $W$ to implement approximate reflection:
$$R \approx 2\Pi_{\rm good} - I$$
where $\Pi_{\rm good}$ projects onto span of $W$-eigenvectors with [[Eigenphase]] $\approx 0$ (near-stationary, unmarked directions).

[[QPE]] with $t = O(\log(1/\sqrt\delta))$ clock [[Qubits]] distinguishes [[Eigenphase]] $0$ (stationary, $M=0$ space) from [[Eigenphase]] $|\theta| \geq \sqrt\delta$ - sufficient to build the reflection. Cost per reflection application:
$$O\!\left(\mathcal{S} + \frac{C}{\sqrt\delta}\right)$$
The reflection identifies marked vertices as the "good" subspace (those with $\theta \approx \sqrt\epsilon$ shift from unmarked stationary phase).
### Amplitude amplification
Apply [[Grover]]-like amplitude amplification with $R$ as [[Oracle]] reflection, repeated $O(1/\sqrt\epsilon)$ times:

**Total MNRS complexity**:
$$T = S + \frac{1}{\sqrt{\epsilon}}\!\left(C + \frac{U}{\sqrt{\delta}}\right)$$
Compare to classical random walk: $O\!\left(\frac{1}{\delta\epsilon}\right)$ steps. Quantum speedup: $O(1/\sqrt\delta)$ from walk mixing, $O(1/\sqrt\epsilon)$ from amplitude amplification.

**[[Quantum counting]]** as preprocessing: run [[QPE]] on $W$ initialized to $\sum_x\sqrt{\pi_x}|x\rangle|p_x\rangle$ to estimate $\epsilon$ before amplitude amplification - calibrates num of amplification rounds when $M$ unknown.

| Problem               | Walk graph               | $\delta$           | $\epsilon$         | Quantum                    | Classical    |
| --------------------- | ------------------------ | ------------------ | ------------------ | -------------------------- | ------------ |
| Element distinctness  | Johnson $J(N,\,N^{2/3})$ | $\Theta(N^{-2/3})$ | $\Theta(N^{-1/3})$ | $O(N^{2/3})$               | $O(N)$       |
| [[Matrix]] product verify | Johnson $J(N,\,N^{2/3})$ | $\Theta(N^{-2/3})$ | $\Theta(N^{-1/3})$ | $O(N^{5/3})$               | $O(N^2)$     |
| Triangle finding      | Johnson $J(N,\,N^{2/3})$ | $\Theta(N^{-2/3})$ | $\Theta(N^{-1/3})$ | $O(N^{5/4})\to O(N^{1.3})$ | $O(N^{3/2})$ |
| Group commutativity   | Cayley graph             | $\Omega(1/N)$      | $\Omega(1)$        | $O(N^{1/2})$               | $O(N)$       |
**Element distinctness** (Ambainis $2004$): given $N$ values $x_1,\ldots,x_N$, decide if any $2$ are equal. Walk on Johnson graph $J(N,r)$ with $r = N^{2/3}$: each vertex = subset $S \subseteq [N]$ of size $r$ loaded into data structure. Marked vertex = $S$ contains collision $x_i = x_j$. Update = [[SWAP]] $1$ element, cost $O(1)$. Total: $O(N^{2/3})$, matches query lower bound - **optimal**.

**Triangle finding**: does input graph on $N$ nodes contain triangle? Improved from $O(N^{5/4})$ (original MNRS) to $O(N^{1.3})$ via refined walk on product graph combining adjacency & neighborhood structure.

**[[Matrix]] product verification**: given $A,B,C \in \mathbb{F}^{N\times N}$, verify $AB = C$ without computing $AB$. Classical Freivalds: $O(N^2)$ randomized. MNRS: $O(N^{5/3})$ - walk on $N^{2/3}$-row sampled subproblem.
### Relation to Grover
[[Grover]] search is special case of MNRS on **complete graph**: $\delta = 1$ (instant mixing), $U = O(1)$ (no data structure), $C = O(1)$ (direct check) $→$ $T = O(1/\sqrt\epsilon) = O(\sqrt N)$. MNRS is strictly $>$ general.
### Unified framework (Apers 2021)
Apers gives single-shot MNRS variant handling multiple solutions without reduction to unique-solution case:
- Removes need to pre-condition on $M = 1$
- Directly applies to element distinctness & triangle finding with multiple collisions
- Chains walks: reuse walk state after outputting $1$ collision, continue for $>$ (relevant for collision-finding algorithms, $2025$)
[Apers 2021: A Unified Framework of Quantum Walk Search (STACS)](https://drops.dagstuhl.de/storage/00lipics/lipics-vol187-stacs2021/LIPIcs.STACS.2021.6/LIPIcs.STACS.2021.6.pdf)
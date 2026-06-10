#Algorithm #Math
**Quantum walks** are the quantum analogue of random walks. [[QPE]] applied to quantum walk operator extracts the **spectral gap** - the key quantity that determines mixing time & enables quantum speedups in search, element distinctness, & triangle finding.
### $2$ models of quantum walks
1. **Discrete-time (Szegedy walk)**: walk operator $W$ acts on bipartite graph edge space. Eigenvalues of $W$ are $e^{\pm i\theta_k}$ where $\cos\theta_k$ relates to the eigenvalues of the classical walk [[Matrix]].
2. **Continuous-time**: evolve under $e^{-iAt}$ where $A$ is the adjacency or Laplacian [[Matrix]]. This is directly [[QPE]] on $e^{-iAt}$ - exactly the chemistry application with a graph Hamiltonian.
### Spectral gap & search
For random walk on $N$ nodes with $M$ marked nodes:
- Spectral gap $\delta = \lambda_2(\text{classical walk})$
- Quantum walk finds marked node in $O(1/\sqrt{\delta})$ steps (vs classical $O(1/\delta)$)
[[QPE]] estimates $\theta$ from the walk [[Eigenvalue]] $e^{i\theta}$. If $\theta \approx 0$, the walk is near-uniform (slow mixing); large $\theta$ means fast mixing.

| Algorithm                   | Walk on           | [[QPE]] extracts   | Speedup                      |
| --------------------------- | ----------------- | ------------------ | ---------------------------- |
| Element distinctness        | Johnson graph     | Marked elements    | $O(N^{2/3})$ vs $O(N)$       |
| Triangle finding            | Johnson graph     | Triangle indicator | $O(N^{5/4})$ vs $O(N^{3/2})$ |
| [[Matrix]] product verification | Product graph     | Witness            | $O(N^{5/3})$ vs $O(N^2)$     |
| Welded trees                | Welded tree graph | Exit node          | Exponential speedup          |
[[Grover]] search is special case: [[Grover]] iterate $G = 1$ step of walk on the complete bipartite graph.

|                        | Discrete (Szegedy)    | Continuous                |
| ---------------------- | --------------------- | ------------------------- |
| Evolution              | Walk operator $W^k$   | $e^{-iAt}$                |
| [[QPE]] target         | $W$                   | $e^{-iAt}$                |
| Spectral info          | Walk eigenvalues      | Laplacian eigenvalues     |
| Relation to [[Grover]] | Direct generalization | Via adjacency [[Matrix]]  |
| Implementation         | Reflection + [[SWAP]] | Trotter / product formula |

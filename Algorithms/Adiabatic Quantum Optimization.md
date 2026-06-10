#Algorithm #Math #Physics
**Adiabatic Quantum Optimization (AQO)** solves combinatorial optimization problems by encoding the solution as the ground state of problem Hamiltonian $H_P$, then using the **adiabatic theorem** to evolve there from an easy-to-prepare ground state.
### Adiabatic theorem
If a quantum system starts in the ground state of $H(0)$ and $H(t)$ changes **slowly enough**, the system stays in the instantaneous ground state throughout. Formally, the condition is:
$$\frac{\max_{t}|\langle E_1(t)|\dot{H}|E_0(t)\rangle|}{\Delta(t)^2} \ll 1$$
where $\Delta(t) = E_1(t) - E_0(t)$ is the **spectral gap** (see [[Eigenvalue]]). Slower evolution is needed when the gap is small.

System evolves under the interpolated Hamiltonian:
$$H(s) = (1-s)\,H_B + s\,H_P \qquad s = t/T \in [0,1]$$

| Component | Role | Typical choice |
|-----------|------|----------------|
| $H_B$ (driver / mixer) | easy ground state, non-commuting with $H_P$ | $-\sum_i X_i$ (transverse field) |
| $H_P$ (problem) | encodes cost function as energy | diagonal in $Z$ basis ([[QUBO Constraints]]) |
| $T$ (total time) | must satisfy adiabatic condition | $T = O(\Delta_{\min}^{-2})$ |
At $s=0$: ground state of $H_B$ is $\vert{+}\rangle^{\otimes n}$ (all $X$ eigenstates) - easy to prepare.
At $s=1$: ground state of $H_P$ = optimal solution.
### QUBO encoding
Most combinatorial problems map to **Quadratic Unconstrained Binary Optimization**:
$$H_P = \sum_i h_i Z_i + \sum_{i<j} J_{ij} Z_i Z_j$$
Variables $x_i \in \{0,1\}$ correspond to qubit states via $x_i = (1 - Z_i)/2$.

Examples:
- **Max-Cut**: $J_{ij} = -1$ for edges $(i,j)$, $h_i = 0$
- **Number partitioning**: encode equal-sum constraint as penalty term
- **Portfolio optimization**: minimize risk - return subject to budget

See [[D-Wave]] hardware for physical QUBO solvers.
### Complexity & gap
Runtime scales as $T = O(\Delta_{\min}^{-2})$ where $\Delta_{\min} = \min_s \Delta(s)$. The minimum gap is the bottleneck:
- For easy instances: $\Delta_{\min} = \Omega(1/\text{poly}(n))$ → polynomial time
- For NP-hard instances: $\Delta_{\min}$ can be exponentially small → exponential time

AQO does **not** provide general polynomial speedup for NP-hard problems, but can offer constant-factor or polynomial speedups on structured instances.
### Relation to other algorithms

| Algorithm | Relationship |
|-----------|-------------|
| [[Quantum annealing]] | Heuristic, non-unitary (open system) special case of AQO; requires stoquastic $H$ |
| [[QAOA]] | Trotterized (digital) approximation of adiabatic evolution at fixed depth $p$ |
| [[VQA]] | Variational generalization; AQO is a specific ansatz trajectory |
| [[QPE]] | Can extract ground energy of $H_P$ to verify AQO result |
### Limitations
- Mini gap $\Delta_{\min}$ is hard to compute classically → hard to certify runtime
- Physical implementations ([[D-Wave]]) use [[Quantum annealing]] (open system, stoquastic) - not full AQO
- Coherence time must exceed $T$ - challenging for large $n$ on current hardware
- No proven quantum advantage over classical simulated annealing for general [[QUBO]]

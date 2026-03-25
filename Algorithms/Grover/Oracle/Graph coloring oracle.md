#Algorithm #Quantum #Graph
**Graph coloring problem**: given an undirected graph $G = (V, E)$ with $|V| = n$ vertices & a palette of $K$ colors, find **proper coloring** - an assignment $c: V \to \{0,\ldots,K-1\}$ such that no ${} 2$ adjacent vertices share color:
$$\forall (u,v) \in E : c(u) \neq c(v)$$
NP-complete for $K \geq 3$. [[Grover]] algorithm searches the $K^n$ possible colorings.

**Encoding**: use $\lceil \log_2 K \rceil$ [[Qubits]]/vertex to represent color. Total input register size: $n \lceil \log_2 K \rceil$ [[Qubits]]. Each basis state $|x⟩ = |c_1 c_2 \cdots c_n⟩$ represents on$1$ complete coloring.

**[[Oracle]] construction**:
1. **Edge [[Oracle]]**: for each edge $(u,v) \in E$, build $U_{(u,v)}$ that marks (flips [[Ancilla]]) when $c_u = c_v$ (edge violated - same color on both endpoints). Equality check: $c_u = c_v \Leftrightarrow c_u \oplus c_v = 0$, implemented with XOR gates & a multi-controlled NOT.
2. **Combine edge oracles**: allocate $1$ [[Ancilla]] qubit per edge. Apply all . The coloring is valid iff no edge [[Ancilla]] was flipped (all ). Apply a multi-controlled NOT to an output qubit conditioned on all edge ancillas being . Uncompute edge ancillas.
3. **Phase kickback**: set output qubit to $|{-}⟩$, apply the marking [[Oracle]], uncompute. Valid colorings receive phase $-1$:
$$U_{\text{color}}^{\text{phase}}:|x⟩ \mapsto (-1)^{[\text{valid}(x)]}|x⟩$$
**[[Grover search]]**: with the above phase [[Oracle]], run [[Grover]] algorithm for $k^* \approx \frac{\pi}{4}\sqrt{K^n / M}$ iterations, where $M$ = num of valid colorings. As with SAT, $M$ may be unknown - use [[Quantum counting]] or exponential search from [[Grover solutions]].

**Resource count**: $|E|$ [[Ancilla]] [[Qubits]] for edge checks + 1 output qubit. Each [[Grover]] iteration: $O(|E|)$ gate cost.

**Special cases**: 2-coloring (bipartiteness test) is classically solvable in $O(|V|+|E|)$; [[Grover]] provides no speedup there. Speedup is meaningful for $K \geq 3$.
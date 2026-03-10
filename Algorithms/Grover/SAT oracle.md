#Algorithm #Quantum 
**SAT (bool Satisfiability)** is the problem of finding assignment of bool variables $x_1,\ldots,x_n \in \{0,1\}$ that satisfies given bool formula $\phi(x_1,\ldots,x_n) = 1$.

[[Grover's algorithm]] solves SAT by encoding $\phi$ as a phase [[Oracle]] & searching the $2^n$ possible assignments.

**CNF (Conjunctive Normal Form)** - standard representation for SAT oracles.
$$\phi = C_1 \wedge C_2 \wedge \cdots \wedge C_m$$
Each **clause** $C_j$ is disjunction of **literals** (variable or its negation):
$$C_j = l_{j,1} \vee l_{j,2} \vee \cdots \vee l_{j,k}$$
where $l_{j,i} \in \{x_r, \neg x_r\}$ for some $r$. Clause is **satisfied** iff min $1$ literal is true. Whole formula is satisfied iff all clauses are satisfied.

**Quantum [[Oracle]] construction for SAT**:
1. **Clause oracles**: for each clause $C_j$, build $U_{C_j}$ that marks (flips [[Ancilla]]) when $C_j = 0$ (clause violated). Clause is **violated** iff _all_ its literals are false.
2. **Combine clauses**: introduce $1$ [[Ancilla]] qubit per clause. Apply $U_{C_1},\ldots,U_{C_m}$ in sequence. Apply a multi-controlled NOT targeting output qubit, conditioned on all clause ancillas being $|0⟩$ (all violated = formula unsatisfied). Invert: flip output when all clauses **satisfied**. Uncompute clause ancillas.
3. **Phase kickback**: set output qubit to $|{-}⟩$ so that satisfying assignments receive $(-1)$ phase. Uncompute output qubit.

Net effect:
$$U_\phi^{\text{phase}}:|x⟩ \mapsto (-1)^{\phi(x)}|x⟩$$
**Unknown solution count $M$**: SAT problems may have $0$, $1$, or many satisfying assignments unknown in advance. Use exponential search or [[Quantum counting]] from [[Grover solutions]] to handle this.

**Complexity**: each Grover iteration calls $U_\phi$ once. $U_\phi$ costs $O(m \cdot k)$ gates (polynomial in formula size). Total: $O(\sqrt{2^n/M} \cdot mk)$ compared to classical $O(2^n)$ exhaustive search - **exponential speedup** for unstructured input.

**Reduction from NP**: since SAT is NP-complete, Grover $+$ SAT [[Oracle]] gives a generic $O(2^{n/2})$ quantum algorithm for any NP problem (via polynomial reduction to SAT). Does not prove $\text{NP} \subseteq \text{BQP}$, but gives quadratic speedup over brute force.
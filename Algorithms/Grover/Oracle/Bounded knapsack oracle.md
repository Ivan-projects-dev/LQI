#Algorithm #Quantum #Optimization
**Bounded knapsack problem**: given $n$ item types, each with weight $w_i$, value $v_i$, & max copy count $b_i$, & a knapsack capacity $W$, find quantities $q_i \in \{0,1,\ldots,b_i\}$ that:
$$\text{maximize } \sum_{i=1}^n v_i q_i \quad \text{subject to} \quad \sum_{i=1}^n w_i q_i \leq W$$
NP-hard in general. [[Grover]] algorithm is applied as a **threshold search**: given a target value $V^*$, find any assignment with total value $\geq V^*$ within capacity $W$.

**Encoding**: represent each $q_i$ with $\lceil \log_2(b_i+1) \rceil$ [[Qubits]]. Total input register size: $\sum_i \lceil \log_2(b_i+1) \rceil$ [[Qubits]]. Each basis state $|x⟩ = |q_1 q_2 \cdots q_n⟩$ encodes $1$ assignment.

**[[Oracle]] construction**:
1. **Weight check**: compute $W_{\text{total}} = \sum_i w_i q_i$ into an [[Ancilla]] register using quantum adders. Mark the state if $W_{\text{total}} \leq W$ (feasible).
2. **Value check**: compute $V_{\text{total}} = \sum_i v_i q_i$ into another [[Ancilla]] register. Mark if $V_{\text{total}} \geq V^*$ (meets threshold).
3. **Combine**: solution is valid iff both checks pass. Apply a Toffoli conditioned on both [[Ancilla]] flags, targeting output qubit. Uncompute [[Ancilla]] registers.
4. **Phase kickback**: prepare output qubit as $|{-}⟩$; valid assignments receive $-1$ phase.
$$U_{\text{knap}}^{\text{phase}}:|x⟩ \mapsto (-1)^{[\text{feasible}(x) \wedge V(x) \geq V^*]}|x⟩$$
**[[Grover]]-based optimization loop**:

Since the target value $V^*$ is unknown, iterate:
1. Set $V^* = 0$. Run [[Grover search]] to find any feasible solution $x_0$.
2. Set $V^* = V(x_0) + 1$. Run [[Grover search]] for a better solution.
3. Repeat until no solution is found. Last found $x$ is optimal.

Each round uses $O(\sqrt{S/M})$ [[Oracle]] calls where $S$ = search space size, $M$ = solutions at current threshold.

**Arithmetic circuit cost**: quantum addition with carry uses $O(n \log b_{\max})$ Toffoli gates per adder. Total [[Oracle]] depth: $O(n \log b_{\max} \log W)$.

**Comparison with 0/1 knapsack**: the bounded variant generalizes 0/1 knapsack (set all $b_i = 1$). The SAT [[Oracle]] approach also applies, but direct arithmetic encoding is $>$ efficient in practice.
#FTQC #ErrorCorrection
**Magic state distillation** is the standard method for fault-tolerantly implementing the non-Clifford **T gate** (and its relatives) on a [[Logical qubit|logical qubit]]. It is typically the dominant resource cost in fault-tolerant quantum computation.

### Why T gates can't be transversal
The **Eastin-Knill theorem** states no quantum error-correcting code can implement a universal gate set transversally. For the [[Surface Code]], Clifford gates (H, S, CNOT) are transversal — applying the physical gate on each qubit implements the logical gate. But the T gate is not transversal: applying $T$ to each physical qubit does **not** produce the logical $T$.

The workaround: prepare a special resource state (magic state) offline, then teleport the T gate into the computation.

### The magic state
$$|T\rangle = T|{+}\rangle = \frac{1}{\sqrt{2}}\left(|0\rangle + e^{i\pi/4}|1\rangle\right)$$

Given one copy of $|T\rangle$ and one qubit in state $|\psi\rangle$, gate teleportation implements $T|\psi\rangle$ using only Clifford operations + measurement. So the T gate cost is entirely offloaded to: **how do you prepare $|T\rangle$ fault-tolerantly?**

### Distillation protocol
Magic state distillation takes $n$ noisy copies of $|T\rangle$ (each with error rate $p$) and outputs $k < n$ higher-fidelity copies.

**15-to-1 protocol** (Bravyi & Kitaev 2005):
- Input: 15 noisy $|T\rangle$ with error rate $p$
- Encode with Reed-Muller $[[15,1,3]]$ code
- Measure stabilizers to detect errors
- Output: 1 distilled $|T\rangle$ with error rate $\sim 35p^3$
- Overhead: 15× more magic states consumed per output

### Error suppression
Each round of distillation cubes the error rate:
$$p_{\text{out}} \approx 35 p_{\text{in}}^3$$

Multiple rounds can be concatenated. For $p_{\text{in}} = 10^{-3}$ after one round: $p_{\text{out}} \approx 35 \times 10^{-9} \approx 3.5 \times 10^{-8}$.

### Distillation protocols comparison
| Protocol | Inputs | Outputs | Output error | Use case |
|---|---|---|---|---|
| 15-to-1 (Bravyi-Kitaev) | 15 | 1 | $35p^3$ | Standard T gate |
| 20-to-4 | 20 | 4 | $O(p^3)$ | Higher throughput |
| $|CCZ\rangle$ factory | — | 1 CCZ | — | Toffoli-heavy circuits |
| Multilevel | nested | 1 | $O(p^{3^k})$ | Ultra-low error |

### Resource cost
Magic state factories consume **50–90% of total physical qubit budget** in early FTQC architectures. A single T-gate factory requires ~100–1000 physical qubits (depending on code distance and distillation level).

In the **Azure Quantum Resource Estimator**, the `tFactories` field reports the number of T factories required; `tGateCount` (from Clifford+T decomposition) drives the total cost.

### Connection to T-count optimization
Reducing the T-count of a circuit directly reduces:
- Number of magic states consumed
- Number of factories required (or factory runtime)
- Total physical qubit cost

This is why T-count is the primary optimization target for fault-tolerant algorithms (see [[QRE]]).

Source: [Bravyi & Kitaev — "Universal quantum computation with ideal Clifford gates and noisy ancillas" (2005)](https://arxiv.org/abs/quant-ph/0403025) [Litinski — "Magic State Distillation: Not as Costly as You Think" (2019)](https://arxiv.org/abs/1905.06903)

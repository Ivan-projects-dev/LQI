#Algorithm #Physics
**Early fault-tolerant (early-[[FTQC]])** regime: quantum error correction can suppress most but not all errors; qubit budgets constrained to $\sim 10^3$–$10^4$ physical [[Qubits]]; code distances low. Standard [[QPE]] requires exponentially deep circuits incompatible with this setting. Several [[QPE]] variants are designed specifically for it.

**Standard [[QPE]]** with $t$ clock [[Qubits]] applies controlled-$U^{2^{t-1}}$ in the final stage - circuit depth exponential in precision. On hardware with physical error rate $p \sim 10^{-3}$ & code distance $d$, logical error rate $\sim (p/p_{\rm th})^{(d+1)/2}$. Limited code distance $→$ residual logical errors corrupt the long $U^{2^k}$ blocks. Circuit depth must stay below $\sim 1/p_{\rm logical}$ for reliable operation.

**Early-[[FTQC]] constraint**: max reliable circuit depth $\sim 10^3$–$10^5$ gates per round, depending on code distance chosen. Forces a tradeoff: fewer $t$ $→$ less precision per round, but more shots tolerable.
### Single-ancilla QPE
[[Iterative QPE]] (Kitaev method) uses $1$ control qubit & $t$ sequential rounds. Only the $k=t-1$ round (running $U^{2^{t-1}}$) is deep; earlier rounds are shallow. Much shorter **per-round** circuit depth, compatible with early [[FTQC]].

Improvement - Dong, Lin & Tong ($2022$): replace [[Hadamard]]-test circuit with **QSP-based single-qubit [[QPE]]** that estimates $\varphi$ with $O(1/\epsilon)$ total $U$ queries at Heisenberg-limited scaling, using $O(\log(1/\delta))$ repetitions per bit. Does not require knowing a good prior on $\varphi$.
### Randomized Trotter for chemistry
Combine single-[[Ancilla]] [[QPE]] with **partially randomized time evolution** to reduce systematic Trotter error without deeper circuits:
- **qDRIFT**: randomly sample terms from $H = \sum_\ell \alpha_\ell H_\ell$ proportional to $\alpha_\ell$; each term applied as $e^{-iH_\ell\tau}$. Systematic bias $→$ random variance - easier to suppress with repetitions
- **Random-order Trotter**: randomize Trotter term ordering each step; systematic commutator errors cancel in expectation, reducing effective Trotter error by $O(1/\sqrt{n_{\rm steps}})$
- **Tradeoff**: variance of phase estimate increases, requiring more shots, but depth per shot decreases
**Feasibility (Mar $2026$)**: chemically accurate [[QPE]] ($< 1.6\times10^{-3}$ hartree) demonstrated for $\text{H}_2$ (4 spin-orbitals) & small molecules in early-FTQC regime with $< 10^4$ physical [[Qubits]].
### Error mitigation + circuit division
**Circuit division** (Ding et al. 2025, PRX Quantum): split large QPE circuit into $K$ smaller sub-circuits, each run at lower code distance. Choose $K$ to minimize total physical qubit $\times$ time. Sub-circuit outputs combined classically.

**EUMLE (Explicitly Unbiased Max Likelihood Estimation)**: data processing technique that mitigates arbitrary errors in QFT-based QPE. Removes systematic bias introduced by depolarizing noise. Outperforms previous state-of-the-art at low & moderate noise rates.

**Tradeoff**: more sub-circuits $→$ lower required code distance per circuit $→$ $<$ physical [[Qubits]] per logical qubit, but $>$ total shots needed. Optimal $K$ depends on hardware noise model.
### Filtered QPE (FQPE)
When initial overlap $\eta = |\langle\psi_0|\psi\rangle|^2$ is small, standard [[QPE]] succeeds with prob $\eta$ only. **Filtered [[QPE]]** applies spectral filter $f(H)$ to amplify ground-state component before or during [[QPE]]:
$$f(H)|\psi\rangle \approx c_0|\psi_0\rangle + \text{small excited contributions}$$
Success prob improves from $\eta$ to $O(\min(1, \eta \cdot G))$ where $G$ is filter gain. See [[Ground State Filtering]] for filter types.
### Adaptive Windowed QPE (AWQPE)
Uses small "windows" of $w$ clock [[Qubits]] at time, estimating $w$ phase bits per window (Jul $2025$):
$$\text{total rounds} = \lceil t/w \rceil, \quad \text{depth per round} \propto 2^w \cdot \text{cost}(U)$$
Window size $w$ balances parallelism vs circuit depth. $w = 1$ = [[Iterative QPE]]; $w = t$ = standard [[QPE]].

Adaptive component: each window can shift its precision target based on previous window outcomes - avoids wasted bits if early estimates are informative. Particularly effective when $\varphi$ is concentrated near known prior.
### Resource comparison (early-FTQC setting)

| Variant            | [[Ancilla]] [[Qubits]] | Max circuit depth                 | Rounds              | Overhead source            |
| ------------------ | ------------------ | --------------------------------- | ------------------- | -------------------------- |
| Standard [[QPE]]   | $t$                | $O(2^t \cdot \text{cost}(U))$     | $1$                 | Full [[FTQC]] required         |
| [[Iterative QPE]]  | $1$                | $O(2^{t-1} \cdot \text{cost}(U))$ | $t$                 | Sequential, no parallelism |
| AWQPE              | $w$                | $O(2^w \cdot \text{cost}(U))$     | $\lceil t/w \rceil$ | Tunable                    |
| FQPE               | $1$–few            | $O(\text{cost}(U)/\sqrt\eta)$     | many                | Handles poor overlap       |
| Randomized Trotter | $1$                | short (many small steps)          | many                | Statistical Trotter error  |

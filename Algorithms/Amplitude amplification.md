#Algorithm #Math
**Amplitude amplification** is the generalization of [[Grover]]'s algorithm. Where Grover searches an unstructured database by marking solutions with an oracle, amplitude amplification applies the same geometric rotation to any quantum algorithm that produces a "good" subspace with amplitude $a = \sin^2\theta$.

### Core idea
Given any quantum algorithm $\mathcal{A}$ that produces state $|\psi\rangle = \sin\theta|\text{good}\rangle + \cos\theta|\text{bad}\rangle$, amplitude amplification rotates $|\psi\rangle$ in the 2D subspace spanned by $|\text{good}\rangle$ and $|\text{bad}\rangle$ until $|\text{good}\rangle$ has amplitude $\approx 1$.

The operator is:
$$Q = -\mathcal{A} S_0 \mathcal{A}^\dagger S_\chi$$
where $S_\chi$ reflects around the "bad" subspace (oracle marking "good" states) and $S_0$ reflects around $|\psi\rangle = \mathcal{A}|0\rangle$.

After $k$ applications of $Q$:
$$P(\text{good after } k \text{ iterations}) = \sin^2\left((2k+1)\theta\right)$$

Optimal $k \approx \frac{\pi}{4\theta}$ gives $P(\text{good}) \approx 1$.

### Comparison to Grover
[[Grover]] is the special case where $\mathcal{A} = H^{\otimes n}$ (uniform superposition) and $a = M/N$ (fraction of marked items):
$$\theta = \arcsin\sqrt{M/N} \approx \sqrt{M/N} \text{ for small } a$$
$$k_{\text{Grover}} \approx \frac{\pi}{4}\sqrt{N/M}$$

Amplitude amplification generalizes this: $\mathcal{A}$ can be **any** quantum circuit. If $\mathcal{A}$ already produces the good state with probability $a$, amplitude amplification reduces the repetitions from $1/a$ (classical) to $O(1/\sqrt{a})$ (quantum) — the quadratic speedup.

### Exact amplitude amplification
Standard amplitude amplification requires knowing $\theta$ to choose $k$ optimally. **Oblivious amplitude amplification** handles the case where $\theta$ is unknown, at the cost of a small constant overhead. **Fixed-point amplitude amplification** (Yoder et al. 2014) always converges without overshoot, regardless of $\theta$.

### Applications
- **Grover search**: $\mathcal{A} = H^{\otimes n}$, oracle marks solution states
- **[[QPE]]**: used inside quantum counting to estimate the number of solutions
- **[[Quantum RNG]]**: $\mathcal{A}$ prepares a biased coin; amplification makes output uniform
- **[[HHL]]**: amplification boosts the probability of measuring the useful solution register
- **[[QPCA]]**: inner loop uses amplitude amplification to sample principal components

### Quantum speedup summary
| Scenario | Classical | Quantum (amplitude amplification) |
|---|---|---|
| $M$ solutions in $N$ items | $O(N/M)$ queries | $O(\sqrt{N/M})$ queries |
| Success probability $a$ | $O(1/a)$ repetitions | $O(1/\sqrt{a})$ repetitions |

Source: [Brassard, Høyer, Mosca, Tapp — "Quantum amplitude amplification and estimation" (2002)](https://arxiv.org/abs/quant-ph/0005055)

#Algorithm #Quantum
When $M > 1$ solutions exist (i.e. $f(x)=1$ for $M$ inputs), [[Grover]] generalizes with min change - only iteration count $k^*$ adjusts.

**Setup**: $N = 2^n$ total states, $M$ marked states, $M < N/2$.

Uniform superposition $|s⟩ = H^{\otimes n}|0⟩^{\otimes n}$ contains each basis state with amplitude $\frac{1}{\sqrt{N}}$. Relevant angle satisfies: $$\sin\theta = \sqrt{\frac{M}{N}}$$
**Optimal iterations**: $$k^* = \left\lfloor \frac{\pi}{4}\sqrt{\frac{N}{M}} \right\rceil$$
After $k^*$ [[Grover]] steps, measuring yields _any one_ of the $M$ solutions with near-uniform probability over the marked set.

**Case $M \geq N/2$**: classical random sampling finds a solution immediately; [[Grover]]'s speedup vanishes. Algorithm still works but $k^* = 0$ or $1$.

**Unknown $M$** - two strategies:

_Quantum counting_: estimate $M$ using QFT-based phase estimation on the [[Grover]] operator $G$. Eigenvalues of $G$ are $e^{\pm 2i\theta}$, so [[QPE]] returns $\theta$ & thus $M \approx N\sin^2\theta$. See [[Quantum counting]].

_Exponential search (no [[QPE]])_: pick a random $k$ from $\{1,\ldots,\lceil\sqrt{N}\rceil\}$, run $k$ [[Grover]] iterations, measure, & check classically. Repeat. Expected total [[Oracle]] calls: $O(\sqrt{N/M})$.

**Multiple-solution [[Diffusion operator]]** is _identical_ to the single-solution case:
$$D = 2|s⟩\langle s| - I = H^{\otimes n}(2|0⟩\langle 0| - I)H^{\otimes n}$$
No modification is needed - the [[Oracle]] $U_f$ automatically marks all $M$ solutions simultaneously.

**Success probability** after $k^*$ steps $\geq 1 - M/N$, approaching $1$ for $M \ll N$.

See [[Grover geometric]] for the rotation picture & derivation of $k^*$.

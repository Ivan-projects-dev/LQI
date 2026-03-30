#Algorithm #Quantum #Math
[[Grover]] can be visualized geometrically as **rotation** in $2D$ subspace, making the iteration count precise & intuitive.

**$2$ basis vectors** span the relevant subspace:
- $|ω⟩$ = the target (marked) state(s)
- $|s'⟩$ = equal superposition over all **unmarked** states, orthogonal to $|ω⟩$
$$|s'⟩ = \frac{1}{\sqrt{N-M}} \sum_{x: f(x)=0} |x⟩$$
Init state $|s⟩$ produced by $H^{\otimes n}|0⟩^{\otimes n}$ lies in this $2D$ plane at small angle $\theta$ from $|s'⟩$:
$$\sin\theta = \sqrt{\frac{M}{N}} \quad \Rightarrow \quad \theta \approx \sqrt{\frac{M}{N}} \text{ for } M \ll N$$
**Each [[Grover]] iteration = rotation by $2\theta$** toward $|ω⟩$:
- **Phase [[Oracle]]** $U_f$ reflects the state about $|s'⟩$ (flips component along $|ω⟩$).
- **[[Diffusion operator]]** $D = 2|s⟩\langle s| - I$ reflects about $|s⟩$.
- $2$ successive reflections = rotation by $2\theta$.

After $k$ iterations the state is: 
$$|\psi_k⟩ = \cos\!\left(\frac{2k+1}{2}\theta\right)|s'⟩ + \sin\!\left(\frac{2k+1}{2}\theta\right)|ω⟩$$
**Optimal iteration count** to reach near $|ω⟩$ (angle $\approx \pi/2$):
$$k^* = \left\lfloor \frac{\pi}{4}\sqrt{\frac{N}{M}} \right\rceil$$
For the single-solution case ($M=1$): $k^* \approx \frac{\pi}{4}\sqrt{N}$, giving $O(\sqrt{N})$ [[Oracle]] calls.

**Overrotation**: running $> k^*$ iterations rotates past $|ω⟩$, reducing success probability - the algorithm must be stopped at the right step.

**Success probability** after $k^*$ steps:
$$P(\text{success}) = \sin^2\!\left((2k^*+1)\theta\right) \geq 1 - \frac{M}{N}$$
Geometric view confirms [[Grover]] achieves **quadratic speedup** & is **asymptotically optimal** for unstructured search (lower bound: any quantum algorithm needs $\Omega(\sqrt{N/M})$ [[Oracle]] calls).
#Algorithm #DB #Math
**Lov Grover** (1996) gave a quantum algorithm for searching an unsorted database of $N$ items in $O(\sqrt{N})$ [[Oracle]] calls — a provably optimal quadratic speedup over any classical algorithm ($\Omega(N)$ queries required classically).

**Problem statement**: given $f: \{0,1\}^n \to \{0,1\}$, find $x_0$ such that $f(x_0)=1$. The search space has $N=2^n$ elements; $M$ of them are solutions.

**Algorithm outline** using $n$ data [[Qubits]] + $1$ [[Ancilla]] qubit:

1. **Init**: apply $H^{\otimes n}$ to create uniform superposition $|s\rangle = \frac{1}{\sqrt{N}}\sum_x |x\rangle$. Each amplitude is $\frac{1}{\sqrt{N}}$.
2. **Repeat $k^*$ times**:
   - **[[Oracle]]** $U_f$: flips sign of amplitude of every marked state ($|x\rangle \to -|x\rangle$ if $f(x)=1$). Implemented via [[Phase kickback]] — [[Ancilla]] set to $|-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}$.
   - **[[Diffusion operator]]** $D = 2|s\rangle\langle s| - I$: reflects all amplitudes about their mean, amplifying marked states.
3. **Measure**: read out $n$ data [[Qubits]] — yields a solution with high probability.

**Optimal iteration count**:
$$k^* = \left\lfloor \frac{\pi}{4}\sqrt{\frac{N}{M}} \right\rceil$$
For $M=1$: $k^* \approx \frac{\pi}{4}\sqrt{N} \approx 0.785\sqrt{N}$. Running past $k^*$ **reduces** success probability (overrotation). See [[Grover geometric]] for the rotation picture.

**[[Diffusion operator]] circuit** (requires 1 [[Ancilla]]):
- Set [[Ancilla]] to $|-\rangle$: apply $X$, then $H$.
- Apply $H^{\otimes n}$ to data [[Qubits]].
- Apply $X^{\otimes n}$ to data [[Qubits]].
- Apply $(n)$-controlled-NOT with [[Ancilla]] as target.
- Apply $X$ to [[Ancilla]].
- Apply $X^{\otimes n}$ to data [[Qubits]].
- Apply $H^{\otimes n}$ to data [[Qubits]].
- Reset [[Ancilla]]: apply $X$, $H$.

**Multiple solutions** ($M > 1$): same circuit, only $k^*$ changes. If $M \geq N/2$ classical random sampling is faster. For unknown $M$, use [[Quantum counting]] ([[QPE]] on $G$) or exponential search. See [[Grover solutions]].

**Lower bound**: any quantum algorithm for unstructured search requires $\Omega(\sqrt{N/M})$ [[Oracle]] calls (BBBV 1994), so Grover is asymptotically optimal.

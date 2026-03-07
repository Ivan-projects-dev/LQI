#Algorithm #Quantum #Math
**Diffusion operator** $D$ (Grover diffusion / inversion about the mean) is the second component of each Grover algorithm iteration, applied after the phase [[Oracle]]. $$D = 2|s\rangle\langle s| - I$$
where $|s\rangle = H^{\otimes n}|0\rangle^{\otimes n} = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}|x\rangle$ is the uniform superposition & $I$ is the $N \times N$ identity.

**Geometric action**: $D$ performs a reflection of the state [[Vector]] about $|s\rangle$. Combined with the [[Oracle]] reflection about $|s'\rangle$, each full Grover step is a rotation by $2\theta$ toward $|ω\rangle$. ([[Grover geometric]])

**Amplitude interpretation**: if the current state is $|\psi\rangle = \sum_x \alpha_x |x\rangle$ with mean amplitude $\mu = \frac{1}{N}\sum_x \alpha_x$, then after $D$: $\alpha_x \;\mapsto\; 2\mu - \alpha_x$

[[Oracle]]-marked state has amplitude $-a$ (flipped sign), while all others remain $+a$. Mean $\mu$ drops slightly. Inversion about $\mu$ then raises the marked amplitude & lowers the rest - **amplitude amplification**.

**Circuit derivation**:
$$D = 2|s\rangle\langle s| - I = H^{\otimes n}(2|0\rangle\langle 0| - I)H^{\otimes n}$$
since $|s\rangle = H^{\otimes n}|0\rangle$, so $|s\rangle\langle s| = H^{\otimes n}|0\rangle\langle 0|H^{\otimes n}$.

Inner operator $2|0\rangle\langle 0| - I$ flips the sign of every basis state _except_ $|0\rangle$.

**Gate sequence for $D$ on $n$ data [[Qubits]]**:
1. Apply $H^{\otimes n}$ to all data [[Qubits]].
2. Apply $X^{\otimes n}$ to all data [[Qubits]] (maps $|0\rangle \to |1\rangle^{\otimes n}$).
3. Apply $n$-controlled $Z$ (multi-controlled phase flip) - marks the all-$|1\rangle$ state.
4. Apply $X^{\otimes n}$ to data [[Qubits]] (undo step 2).
5. Apply $H^{\otimes n}$ to data [[Qubits]].

Net effect: implements $2|0\rangle\langle 0| - I$ conjugated by $H^{\otimes n}$, yielding $D$. See Diffusion operator in Q-Sharp for Q# code.

**Gate count**: $O(n)$ [[Single-qubit gates]] + $1$ $n$-controlled $Z$ (decomposable into $O(n)$ Toffoli gates with [[Ancilla]]). Total Grover step: $1$ [[Oracle]] call + $O(n)$ gates.

**Unitarity**: $D$ is self-inverse - it is its own adjoint: $D^\dagger = D, \quad D^2 = I$
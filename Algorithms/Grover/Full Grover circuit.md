#Algorithm #Quantum 
Complete structure of [[Grover]] algorithm from qubit allocation to measurement.

**Qubit layout**:
- $n$ **data [[Qubits]]** - encode the search space $\{0,1\}^n$, $N = 2^n$.
- $1$ **[[Ancilla]] qubit** - used by the phase [[Oracle]] (prepared in $|{-}\rangle$) & by diffusion circuit.
- Additional [[Ancilla]] [[Qubits]] may be required by the [[Oracle]]'s internal computation (uncomputed before next iteration).

1. **Initialization**: $|\psi_0\rangle = |0\rangle^{\otimes n} \otimes |0\rangle_{\text{anc}}$
Apply $H^{\otimes n}$ to data [[Qubits]] → uniform superposition. Prepare [[Ancilla]] in $|{-}\rangle$ via $X$ then $H$:
$$|\psi_1\rangle = \frac{1}{\sqrt{N}}\sum_{x=0}^{N-1}|x\rangle \otimes |{-}\rangle$$
2. **[[Grover]] iteration** (repeat $k^*$ times):
a. **Phase [[Oracle]]** $U_f$: marks solution state(s) with phase $-1$ via phase kickback from [[Ancilla]] $|{-}\rangle$. [[Oracle]] is problem-specific (SAT [[Oracle]], Graph coloring [[Oracle]], Bounded knapsack [[Oracle]]).
b. **[[Diffusion operator]]** $D = H^{\otimes n}(2|0\rangle\langle 0| - I)H^{\otimes n}$: reflects amplitudes about the mean. Each full iteration $G = D \cdot U_f$ rotates the state by $2\theta$ toward the solution(s) (see [[Diffusion operator]].

3. **Optimal stop after $k^*$ iterations**:
$$k^* = \left\lfloor \frac{\pi}{4}\sqrt{\frac{N}{M}} \right\rceil$$
For unknown $M$: use [[Quantum counting]] or exponential search (see [[Grover solutions]]).

4. Measure the $n$ data [[Qubits]] in the computational basis. With probability $\geq 1 - M/N$ the result is valid solution $x^*$ satisfying $f(x^*) = 1$. Verify classically: evaluate $f(x^*)$ once to confirm.

**Full circuit (compact notation)**:
$$|\psi_{\text{out}}\rangle = G^{k^*} \cdot (H^{\otimes n} \otimes XH) \cdot |0\rangle^{\otimes n+1}$$
$$G = \left(H^{\otimes n}(2|0\rangle\langle 0|-I)H^{\otimes n}\right) \cdot U_f$$

| Resource            | Cost                                 |
| ------------------- | ------------------------------------ |
| [[Qubits]]              | $n + 1 +$ [[Oracle]] [[Ancilla]]             |
| [[Oracle]] calls        | $O(\sqrt{N/M})$                      |
| Gates per iteration | $O(n) +$ [[Oracle]] cost                 |
| Total gates         | $O(\sqrt{N/M} \cdot (n +$ [[Oracle]]$))$ |
| Classical post-proc | $O(1)$ check                         |

**Optimality**: the $O(\sqrt{N})$ [[Oracle]] complexity is **tight** -  no quantum algorithm can search unstructured DB in $<\Omega(\sqrt{N})$ queries (BBBV lower bound theorem). 
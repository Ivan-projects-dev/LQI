#Algorithm #Quantum #Q-Sharp
[[Grover's algorithm]] uses $2$ kinds of oracles. **Marking [[Oracle]]**: flips target [[Ancilla]] qubit if query register satisfies condition. **Phase [[Oracle]]**: flips phase of query register if condition holds. The two are interconvertible.

**[[Oracle]] types**:
	- **AllOnes** – flip target iff all [[Qubits]] are $|1\rangle$. Implemented by a single multi-controlled $X$ (Toffoli generalization) with all data [[Qubits]] as controls & target as the output qubit.
	- **AlternatingBits** – flip target iff register is in $|10101\ldots\rangle$. Implemented by applying $X$ to every even-indexed qubit (to turn $|10\ldots\rangle$ into $|11\ldots\rangle$), then using the AllOnes [[Oracle]], then uncomputing the $X$s.
	- **ArbitraryPattern** - flip target iff register matches a given bit pattern `Bool[]`. For every qubit where the pattern bit is `false` (i.e. target is $|0\rangle$), apply $X$ before the multi-controlled $X$, then uncompute. This turns the desired state into $|11\ldots1\rangle$ so AllOnes fires.

**[[Oracle]] converter** (marking → phase):
Wrap the marking [[Oracle]] with an [[Ancilla]] prepared in $|{-}\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}$. When the [[Oracle]] fires it flips $|{-}\rangle \to -|{-}\rangle$ (**phase kickback**), leaving the [[Ancilla]] unchanged & writing $-1$ into the query register's phase: $$U_f|x\rangle|{-}\rangle = (-1)^{f(x)}|x\rangle|{-}\rangle$$[[Ancilla]] is never measured & can be reused across iterations.

**Grover iteration – 4 steps** (from [[Oracle]] to diffusion):
1. Apply phase [[Oracle]] $U_f$ → marks solution(s) with $-1$.
2. Apply $H^{\otimes n}$ to query register.
3. Apply **conditional phase flip** $2|0\rangle\langle 0| - I$: flip sign of every state _except_ $|0\ldots0\rangle$ (equivalently, flip only $|0\ldots0\rangle$ & absorb global phase). Circuit: multi-controlled $Z$ (or use [[Oracle]]-converter trick with [[Ancilla]] in $|{-}\rangle$).
4. Apply $H^{\otimes n}$ again.

Steps $2-4$ together form the [[Diffusion operator]] $D = H^{\otimes n}(2|0\rangle\langle 0|-I)H^{\otimes n}$.

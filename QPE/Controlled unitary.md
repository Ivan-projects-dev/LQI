#Algorithm #Quantum #Math 
**Controlled-$U^{2^k}$** is the key building block of [[QPE]]. For each control qubit $j$ ($j = 0, \ldots, t-1$), [[QPE]] requires the operation:
$$C\text{-}U^{2^j}: |c\rangle|u\rangle \mapsto |c\rangle \cdot U^{2^j \cdot c}|u\rangle \quad c \in \{0,1\}$$
i.e., apply $U^{2^j}$ to the target register iff the $jth$ control qubit is $|1\rangle$.

**Repeated squaring**: the sequence $U^1, U^2, U^4, \ldots, U^{2^{t-1}}$ is built by squaring:
$$U^{2^j} = \underbrace{U \circ U \circ \cdots \circ U}_{2^j \text{ times}} = \left(U^{2^{j-1}}\right)^2$$
Direct implementation of $C$-$U^{2^j}$ via $2^j$ sequential apps of $C$-$U$ is correct but costs $O(2^j)$ gates per stage → $O(2^t)$ total, giving the standard [[QPE]] gate count.

**Efficient squaring**: if compact circuit for $U$ is known, $U^2$ can often be compiled directly into similarly-sized circuit, keeping each stage at $O(\text{cost}(U))$ gates. This is problem-dependent.

**Phase kickback perspective**: when the target is in eigenstate $|u\rangle$:
$$C\text{-}U^{2^j}(|{+}\rangle|u\rangle) = \frac{1}{\sqrt{2}}\left(|0\rangle + e^{2\pi i 2^j \varphi}|1\rangle\right)|u\rangle$$
Phase $e^{2\pi i 2^j \varphi}$ is kicked back onto the control qubit. Each control qubit accumulates different power of the phase - the binary digits of $\varphi$ are encoded across the $t$ control [[Qubits]] before the inverse QFT.

**When eigenstates are unknown**: [[QPE]] still works if the target register is initialized to any state $|\psi\rangle = \sum_u c_u |u_u\rangle$ (superposition of eigenstates). Measurement then returns $1$ of the eigenphases $\varphi_u$ with probability $|c_u|^2$.

**Black-box vs structured $U$**: in the [[Oracle]] model (e.g. [[Quantum counting]]), $U$ is black box & $C$-$U^{2^j}$ is implemented by calling the [[Oracle]] $2^j$ times each controlled on qubit $j$ - $O(2^t)$ total [[Oracle]] calls. In structured settings (e.g. quantum chemistry Hamiltonians), $e^{-iHt}$ is approximated via Trotterization, reducing the effective gate count.

**Q# implementation**: use the `Controlled` functor from [[Functors]] - `Controlled applyU([control[j]], (power, eigenstate))`. 

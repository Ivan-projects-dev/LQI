#Math #Algorithm
**Linear Combination of Unitaries (LCU)** expresses an operator $A$ as a weighted sum of unitary operators:
$$A = \sum_{k=0}^{N-1} \alpha_k U_k \qquad \alpha_k \in \mathbb{R},\ \alpha_k > 0,\ U_k \text{ unitary}$$

Since quantum computers can only apply unitary gates directly, LCU is the bridge that lets them implement arbitrary (non-unitary) operators like Hamiltonians.

### Pauli decomposition - the standard LCU
Any $n$-qubit operator $A$ can be expanded in the Pauli basis $\{I, X, Y, Z\}^{\otimes n}$:
$$A = \sum_{P \in \{I,X,Y,Z\}^n} \alpha_P \, P$$
Since all Pauli strings are unitary, this immediately gives an LCU. For molecular Hamiltonians via [[Jordan-Wigner encoding]] or [[Bravyi-Kitaev encoding]], this decomposition has $O(N^4)$ terms for $N$ spin-orbitals.

[[PennyLane]]: `qml.pauli_decompose(A)` returns coefficients and Pauli unitaries.

### PREP and SEL oracles
Given $A = \sum_k \alpha_k U_k$ with $\lambda = \sum_k \alpha_k$:

**PREP** (prepare [[Oracle]]) - loads the LCU coefficients into amplitudes:
$$\text{PREP}|0\rangle = \sum_k \sqrt{\frac{\alpha_k}{\lambda}}|k\rangle$$
Requires $\lceil\log_2 N\rceil$ [[Ancilla]] [[Qubits]]. Implementable with [[Quantum Fourier Transform|QFT]]-based [[State preparation]] or QROM.

**SEL** (select [[Oracle]]) - applies the $k$-th unitary controlled on $|k\rangle$:
$$\text{SEL}|k\rangle|\psi\rangle = |k\rangle\, U_k|\psi\rangle$$
Implementable as a sequence of multi-controlled gates.

### From LCU to block encoding
Combining PREP and SEL gives a [[Block encoding]] of $A$:
$$U = \text{PREP}^\dagger \cdot \text{SEL} \cdot \text{PREP}$$
$$\Rightarrow\quad \langle 0|U|0\rangle|\psi\rangle = \frac{A}{\lambda}|\psi\rangle$$

$A$ appears in the $|0\rangle$ subspace of $U$. Post-selecting $|0\rangle$ on the [[Ancilla]] applies $A/\lambda$ to the system.

### Normalization constant $\lambda$
$$\lambda = \sum_k |\alpha_k| \quad \text{(1-norm of coefficients)}$$
$\lambda$ sets the "scale" of the [[Block encoding]]. All [[QPE]]-via-[[Qubitization]] costs scale as $O(\lambda/\epsilon)$. Minimizing $\lambda$ through smarter decompositions (e.g., factored forms, [[Tensor]] hypercontraction) is an active area in quantum chemistry compilation.

### LCU for Hamiltonian simulation
Time evolution $e^{-iHt}$ can be implemented via LCU using the Jacobi-Anger expansion:
$$e^{-iHt} = \sum_{k=-\infty}^{\infty} (-i)^k J_k(t)\, H^k/k! \approx \text{truncated LCU}$$
or more practically via [[Quantum signal processing]] applied to the block-encoded $H/\lambda$.

### Comparison with Trotterization

| | [[Trotterization]] | LCU / [[Block encoding]] |
|-|-------------------|------------------------|
| [[Ancilla]] [[Qubits]] | None | $O(\log N)$ |
| Error scaling | $O(t^2/n)$ (1st order) | $O(\log(1/\epsilon))$ |
| 1-norm dependence | No ($\|\[H_i,H_j\]\|$) | Yes ($\lambda = \sum \alpha_k$) |
| Implementation | Simple | Complex |
| Best for | Small molecules, shallow circuits | Large systems, [[FTQC]] |

See [[Hamiltonian simulation]] for full comparison. See [[Block encoding]] for the circuit construction.

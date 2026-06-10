#Algorithm #Math
**Block encoding** embeds a (possibly non-unitary) operator $A$ into the top-left block of a larger unitary $U$ acting on a system + [[Ancilla]] register:
$$U = \begin{bmatrix} A/\lambda & \cdot \\ \cdot & \cdot \end{bmatrix}$$
where $\lambda = \sum_k |\alpha_k|$ is a normalization constant. When the [[Ancilla]] is measured in $|0\rangle$, the system state has been acted on by $A$ (up to $\lambda$). This allows non-unitary operations - including Hamiltonians, projectors, and general matrices - to run on a quantum computer.

### Construction via LCU (PREP-SEL-PREP†)
Starting from an [[LCU]] decomposition $A = \sum_{k=0}^{N-1} \alpha_k U_k$ with real positive $\alpha_k$:

**PREP** - prepares the coefficient state on [[Ancilla]]:
$$\text{PREP}|0\rangle = \sum_k \sqrt{\frac{\alpha_k}{\lambda}}|k\rangle$$

**SEL** - selects which unitary acts on the system:
$$\text{SEL}|k\rangle|\psi\rangle = |k\rangle\, U_k|\psi\rangle$$

**Block encoding circuit**:
$$U = \text{PREP}^\dagger \cdot \text{SEL} \cdot \text{PREP}$$

Then $\langle 0|\text{PREP}^\dagger \cdot \text{SEL} \cdot \text{PREP}|0\rangle|\psi\rangle = \frac{A}{\lambda}|\psi\rangle$

Post-selecting on $|0\rangle$ in the [[Ancilla]] succeeds with probability $\|A|\psi\rangle\|^2/\lambda^2$.

### Resource cost

| Resource | Cost |
|----------|------|
| Ancilla qubits | $\lceil\log_2 N\rceil$ (for $N$ LCU terms) |
| PREP circuit | $O(N)$ gates or $O(\log N)$ with QROM |
| SEL circuit | $O(N \cdot \text{cost}(U_k))$ |
| Success probability | $\|A|\psi\rangle\|^2/\lambda^2$ |

For Hamiltonian $H = \sum_k \alpha_k P_k$ (Pauli strings): $N = O(N^4)$ terms from [[Jordan-Wigner encoding]], $\lambda = \sum_k |\alpha_k|$ (1-norm of coefficients).

### Why $\lambda$ matters
The normalization $\lambda$ directly determines QPE precision cost: [[Qubitization]] achieves gate complexity $O(\lambda / \epsilon)$ via block encoding, vs $O(\lambda^2/\epsilon)$ for naive [[Trotterization]]. Minimizing $\lambda$ (tighter Hamiltonian decompositions) is an active research area.

### Relation to other techniques

| Technique | Uses block encoding? | Notes |
|-----------|---------------------|-------|
| [[LCU]] | Directly | [[LCU]] is how PREP+SEL is constructed |
| [[Qubitization]] | Yes | Walk operator $W$ built from block encoding of $H/\lambda$ |
| [[Quantum signal processing]] | Yes | QSP applies polynomial $p(H/\lambda)$ to block-encoded $H$ |
| [[Trotterization]] | No | Alternative approach; no [[Ancilla]] overhead but $O(1/\epsilon)$ scaling |

See [[Hamiltonian simulation]] for comparison of all approaches.

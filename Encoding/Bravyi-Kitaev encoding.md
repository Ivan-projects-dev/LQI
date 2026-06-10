#Algorithm #Chemistry #Math
**Bravyi-Kitaev (BK) encoding** maps fermionic operators to qubit Pauli operators using hierarchical binary tree structure, achieving $O(\log N)$ average Pauli string length vs $O(N)$ for [[Jordan-Wigner encoding]]. This directly reduces circuit depth for [[QPE Chemistry]] and [[VQE]].

Jordan-Wigner stores **occupation** in each qubit. BK stores a mixture of occupation and **parity** information in a way that keeps both update and parity operations local ($O(\log N)$ [[Qubits]] each).

Each qubit $p$ stores information about a set of orbitals determined by the binary tree structure of $p$'s index. Three sets per qubit:
- **Update set** $U(p)$: [[Qubits]] that must be updated when orbital $p$ is occupied/emptied
- **Parity set** $P(p)$: [[Qubits]] needed to compute parity below $p$
- **Remainder set** $R(p)$: subset of $U(p)$ used for anti-commutation correction

All three sets have size $O(\log N)$.
### BK creation operator
$$a_p^\dagger \mapsto \frac{1}{2}\left(\bigotimes_{k\in U(p)} X_k\right)\left(X_p - iY_p\right)\left(\bigotimes_{k\in P(p)} Z_k\right)$$
Compared to Jordan-Wigner's $O(N)$ $Z$-string, BK's $P(p)$ has only $O(\log N)$ terms.

| Property | [[Jordan-Wigner encoding\|Jordan-Wigner]] | Bravyi-Kitaev |
|----------|--------------|--------------|
| [[Qubits]] | $N$ | $N$ |
| Avg Pauli string length | $O(N)$ | $O(\log N)$ |
| Trotter step gates | $O(N)$ per term | $O(\log N)$ per term |
| Circuit depth scaling | $O(N^5)$ (with $O(N^4)$ terms) | $O(N^4 \log N)$ |
| Encoding complexity | Simple | Complex |
| Best for | $N \lesssim 20$ | $N \gtrsim 20$, hardware-efficient |
### Practical impact
For $N = 100$ spin-orbitals:
- JW: strings up to length $100$ → 100 [[CNOT]] gates per term
- BK: strings up to length $\log_2(100) \approx 7$ → ~7 [[CNOT]] gates per term

This is a $\sim 14\times$ reduction in [[CNOT]] count per Trotter step - significant for near-term hardware where [[CNOT]] error dominates.

### Limitations
- BK qubit basis is not the occupation basis - classical pre/post-processing needed to interpret results
- Active qubit ordering matters; naive BK may not match hardware connectivity
- More recent alternatives (e.g., superfast BK, Fenwick tree encoding) improve specific cases

### Workflow position
In [[QPE Chemistry]]:
1. Write molecular Hamiltonian in [[Second quantization]]
2. Apply BK encoding → Pauli string Hamiltonian $H = \sum_j c_j P_j$
3. Use [[Trotterization]] or [[Qubitization]] to implement $e^{-iHt}$
4. Run [[QPE]] to extract ground-state energy $E_0$

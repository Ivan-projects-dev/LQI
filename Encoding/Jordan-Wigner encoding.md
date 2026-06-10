#Algorithm #Chemistry #Math
**Jordan-Wigner (JW) encoding** maps fermionic creation/annihilation operators to qubit Pauli operators. It is the standard first step in [[QPE Chemistry]] and [[VQE]] for simulating molecular Hamiltonians on a quantum computer.

### The problem
Molecular Hamiltonians ([[Second quantization]]) use fermionic operators $a_p^\dagger, a_p$ obeying **anti-commutation**:
$$\{a_p, a_q^\dagger\} = \delta_{pq} \qquad \{a_p, a_q\} = 0$$
[[Qubits]] obey commutation relations - direct mapping is impossible without correction terms.

### Jordan-Wigner mapping
Assign one qubit per spin-orbital. Qubit $p$ stores occupation ($\vert0\rangle$ = empty, $\vert1\rangle$ = occupied):
$$a_p^\dagger \mapsto \left(\bigotimes_{k=0}^{p-1} Z_k\right) \otimes \frac{X_p - iY_p}{2}$$
$$a_p \mapsto \left(\bigotimes_{k=0}^{p-1} Z_k\right) \otimes \frac{X_p + iY_p}{2}$$

The $Z$-string $\bigotimes_{k<p} Z_k$ tracks the **parity** of all lower-indexed orbitals - necessary to enforce anti-commutation.

### Qubit Hamiltonian
Substituting into the molecular Hamiltonian $H = \sum_{pq} h_{pq} a_p^\dagger a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs} a_p^\dagger a_q^\dagger a_r a_s$ yields a **Pauli string Hamiltonian**:
$$H = \sum_j c_j \, P_j \qquad P_j \in \{I,X,Y,Z\}^{\otimes N}$$

| Property | Jordan-Wigner |
|----------|--------------|
| Qubits required | $N$ (one per spin-orbital) |
| Pauli terms | $O(N^4)$ for 2-body interactions |
| String length | Up to $O(N)$ (long $Z$-strings) |
| Gate overhead | High - long $Z$-strings → deep circuits |

### Worked example: H₂ in minimal basis (STO-3G)
4 spin-orbitals → 4 qubits. After JW and tapering symmetries, reduces to 2-qubit problem:
$$H_{H_2} = c_0 I + c_1 Z_0 + c_2 Z_1 + c_3 Z_0 Z_1 + c_4 X_0 X_1 + c_5 Y_0 Y_1$$
Each Pauli string exponential $e^{-iP_j t}$ is a rotation gate - directly implements [[Trotterization]] steps.

### Jordan-Wigner vs Bravyi-Kitaev
[[Bravyi-Kitaev encoding]] reduces average Pauli string length from $O(N)$ to $O(\log N)$, lowering circuit depth at the cost of a more complex encoding:

| | Jordan-Wigner | Bravyi-Kitaev |
|-|--------------|--------------|
| [[Qubits]] | $N$ | $N$ |
| String length | $O(N)$ avg | $O(\log N)$ avg |
| Implementation | Simple | More complex |
| Use case | Small molecules, pedagogy | Larger systems |

### Limitations
- $O(N)$ Pauli string length → $O(N)$ gates per Trotter step per term
- $O(N^4)$ terms → quadratic scaling bottleneck for large molecules
- Active area: compact encodings, symmetry tapering, and fermion-to-qubit mappings (e.g., Bravyi-Kitaev superfast, generalized superfast)

See [[QPE Chemistry]] for the full simulation workflow. See [[Trotterization]] for how Pauli strings become circuit gates.

#Physics #Chemistry #Math
**Second quantization** is the mathematical framework for describing many-body quantum systems (electrons, photons, etc.) using **creation and annihilation operators** rather than explicit many-particle wave functions. It is the starting point for all quantum chemistry on quantum computers.

### Fock space
Instead of tracking individual particle positions, second quantization works in **Fock space** - the direct sum of Hilbert spaces with different particle numbers:
$$\mathcal{F} = \mathcal{H}_0 \oplus \mathcal{H}_1 \oplus \mathcal{H}_2 \oplus \cdots$$
Each spin-orbital $p$ (a spatial orbital × spin label) can be **empty** ($|0\rangle_p$) or **occupied** ($|1\rangle_p$). A many-electron state is specified by its occupation pattern:
$$|n_0, n_1, n_2, \ldots, n_{N-1}\rangle \quad n_p \in \{0,1\}$$

### Creation and annihilation operators
**Creation operator** $a_p^\dagger$: adds an electron to spin-orbital $p$
$$a_p^\dagger |n_0,\ldots,0_p,\ldots\rangle = (-1)^{\sum_{q<p}n_q} |n_0,\ldots,1_p,\ldots\rangle$$

**Annihilation operator** $a_p$: removes an electron from spin-orbital $p$
$$a_p |n_0,\ldots,1_p,\ldots\rangle = (-1)^{\sum_{q<p}n_q} |n_0,\ldots,0_p,\ldots\rangle$$

The $(-1)$ **parity factor** encodes the anti-symmetry of fermionic wave functions. It counts the number of occupied lower-indexed orbitals, flipping sign when this is odd.

### Canonical anti-commutation relations (CAR)
$$\{a_p, a_q^\dagger\} = a_p a_q^\dagger + a_q^\dagger a_p = \delta_{pq}$$
$$\{a_p, a_q\} = \{a_p^\dagger, a_q^\dagger\} = 0$$

Key consequences:
- $(a_p^\dagger)^2 = 0$ - **Pauli exclusion principle**: can't add two electrons to the same orbital
- $a_p^\dagger a_p$ = occupation number operator: [[Eigenvalue]] $0$ or $1$
- Operators on different orbitals anti-commute: $a_p a_q^\dagger = -a_q^\dagger a_p$ for $p \neq q$

### Molecular Hamiltonian in second quantization
The electronic Hamiltonian (Born-Oppenheimer approximation):
$$H = \underbrace{\sum_{p,q} h_{pq}\, a_p^\dagger a_q}_{\text{1-body (kinetic + nuclear)}} + \underbrace{\frac{1}{2}\sum_{p,q,r,s} h_{pqrs}\, a_p^\dagger a_q^\dagger a_r a_s}_{\text{2-body (electron-electron repulsion)}}$$

The **integrals** $h_{pq}$ and $h_{pqrs}$ are computed classically (Hartree-Fock, CCSD, etc.) and encode the nuclear positions and charges.

For $N$ spin-orbitals: $O(N^2)$ one-body terms, $O(N^4)$ two-body terms.

### Mapping to qubits
Second-quantized operators don't directly act on [[Qubits]] - the anti-commutation relations must be preserved. Two standard mappings:

| Property | [[Jordan-Wigner encoding]] | [[Bravyi-Kitaev encoding]] |
|----------|--------------------------|--------------------------|
| Qubit per orbital | 1 | 1 |
| Parity string length | $O(N)$ | $O(\log N)$ |
| Result | Pauli string Hamiltonian | Pauli string Hamiltonian |

After mapping: $H = \sum_j c_j P_j$ where $P_j \in \{I,X,Y,Z\}^{\otimes N}$. This Pauli form is fed into [[Trotterization]], [[LCU]], or [[Qubitization]] for [[QPE]].

### Number of Pauli terms
After Jordan-Wigner mapping:
- Minimal basis H₂ (4 spin-orbitals): ~15 Pauli terms → tractable classically
- Minimal basis N₂ (20 spin-orbitals): ~2,951 terms
- FeMoco (56 spin-orbitals, active space): ~150,000+ terms → requires quantum advantage

### Connection to QPE Chemistry
[[QPE Chemistry]] workflow:
1. Compute integrals $h_{pq}, h_{pqrs}$ classically → second quantized $H$
2. Apply [[Jordan-Wigner encoding]] or [[Bravyi-Kitaev encoding]] → Pauli $H$
3. Implement $e^{-iHt}$ via [[Trotterization]] or [[Qubitization]]
4. Run [[QPE]] with trial state ([[State preparation]])
5. Measure → ground state energy $E_0$

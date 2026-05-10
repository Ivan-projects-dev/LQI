#Algorithm #Physics #Chemistry
**[[QPE]] in [[Quantum simulation]]** extracts spectral properties (energy eigenvalues, gaps, phase diagrams) of physical systems beyond molecular chemistry. Provides the fault-tolerant route to exponential speedup over classical diagonalization for systems where classical methods fail (sign problem, exponential Hilbert space, strong correlations).

Physical system $→$ Hamiltonian $H$ $→$ qubit encoding $→$ implement $e^{-iHt}$ (via [[Trotter formula|Trotterization]] or [[Qubitization]]) $→$ [[QPE]] extracts eigenvalue $E_j$.

**Energy resolution**: $\epsilon = 2\pi/(\tau \cdot 2^t)$ where $\tau$ = total evolution time, $t$ = clock [[Qubits]]. Improving resolution by $2\times$ costs $2\times$ more $U$ calls.

**Qubit encoding** of physical degrees of freedom varies by system:
- Fermions: Jordan-Wigner (1D: $O(n)$ locality), Bravyi-Kitaev ($O(\log n)$ locality), or first-quantized plane-wave basis
- Bosons: truncated Fock space, unary or binary encoding of occupation numbers
- Gauge fields: plaquette operators, link variables - higher-dimensional Hilbert space per site
### Lattice gauge theories
**Gauge theories** describe fundamental forces (Standard Model: $U(1)$ QED, $SU(2)$ weak, $SU(3)$ QCD). Classical lattice QCD at finite baryon density $\mu \neq 0$ fails due to the **sign problem** - Monte Carlo importance sampling breaks down. [[Quantum simulation]] has no sign problem.

**Kogut-Susskind Hamiltonian** on $L^d$ lattice:
$$H = H_E + H_B + H_{\rm fermion} + H_{\rm Yukawa}$$
- $H_E$: electric field energy (diagonal in link basis)
- $H_B$: magnetic field energy (plaquette operators - non-commuting)
- $H_{\rm fermion}$: hopping terms for matter fields

[[QPE]] on $e^{-iHt}$ with trial vacuum state $→$ extracts **mass gap** (lightest particle mass $\Delta E = E_1 - E_0$), **string tension** (confinement order parameter), **phase diagram** of confinement/deconfinement transition.

**Recent results**: $Z_2$ lattice gauge theory confinement tested on Google's quantum AI hardware (Nature Physics, Jan 2025). $U(1)$ QED in $(2+1)$D simulated on trapped-ion qudits (Nature Physics, 2025) - [[Qubits]] encode gauge field occupations $>$ efficiently than [[Qubits]].

**Qubit count**: scales with lattice volume $V = L^d$ & gauge group dimension. $SU(2)$ in $(2+1)$D: $\sim 10^2$–$10^3$ logical [[Qubits]] for small classically-hard instances; $SU(3)$ in $(3+1)$D: estimates $\sim 10^4$–$10^6$ logical [[Qubits]] for beyond-classical regime.

### Hubbard model
Strongly correlated electrons on lattice:
$$H = -t\sum_{\langle i,j\rangle,\sigma}\!\!(c^\dagger_{i\sigma}c_{j\sigma} + h.c.) + U\sum_i n_{i\uparrow}n_{i\downarrow}$$
Classical simulation fails in regime $t \sim U$ - the Mott transition. Best classical methods (quantum Monte Carlo) have sign problem for frustrated geometries or doped systems relevant to high-$T_c$ superconductivity.

**[[QPE]] targets**:
- Ground-state energy vs $U/t$ $→$ locate **Mott transition** (energy cusp at critical $U/t$)
- Spectral function $A(k,\omega)$ $→$ map metallic vs Mott insulator phases
- Pairing correlations in doped Hubbard $→$ superconducting order parameter

**Qubit count**: $2n$ [[Qubits]] for $n$-site lattice (spin-up/down per site). 2D $L\times L$ lattice: $2L^2$ [[Qubits]] + clock register. Classically intractable at $L \sim 10$ ($200$ [[Qubits]]).

[[Qubitization]] of Hubbard: $\lambda = O(tn + Un)$ exact 1-norm; T-gate count $\sim O(\lambda/\epsilon)$ - competitive with chemistry targets.

### Condensed matter: band structure & topology

**Band structure**: crystalline Hamiltonian $H(\mathbf{k})$ at each Bloch momentum $\mathbf{k}$. [[QPE]] on $e^{-iH(\mathbf{k})t}$ extracts energy bands $E_n(\mathbf{k})$.

Applications:
- **Topological invariants**: Chern numbers computed from Berry phases $\gamma_n = i\oint\langle u_n|\nabla_\mathbf{k}|u_n\rangle\cdot d\mathbf{k}$ - [[QPE]] enables exact eigenstate access needed for Berry phase
- **Topological phase transitions**: [[QPE]] detects spectral gap closing at transition points - exponentially small gap near criticality requires high precision $\epsilon$
- **Phonon spectra**: [[QPE]] on lattice dynamics Hamiltonian $→$ phonon dispersion curves
- **Magnon spectra**: spin-wave excitations from magnetic Hamiltonians

**Symmetry-protected topological (SPT) phases**: ground-state degeneracy on finite systems $→$ [[QPE]] distinguishes near-degenerate ground states by resolving exponentially small energy splitting $\Delta E \sim e^{-L}$.

### Nuclear physics
Nuclear structure: $A$-body Hamiltonian from chiral effective field theory (EFT) in single-particle basis. Classical **no-core shell model** limited to $A \lesssim 20$ nucleons.

**[[QPE]] targets**:
- Ground-state & excited-state energies of medium-mass nuclei ($A \sim 30$–$100$)
- Binding energy differences (nuclear chart)
- Scattering observables (T-[[Matrix]] eigenvalues $→$ reaction cross-sections)
- Neutrino oscillation Hamiltonians (CP violation)
Qubit encoding: $\sim 50$–$200$ [[Qubits]] per nucleus depending on single-particle basis truncation. First systematic resource estimates published 2024–2025; classically intractable instances estimated at $\sim 10^3$ logical [[Qubits]].

### Comparison across simulation targets

| System            | Classical bottleneck            | [[QPE]] target           | Key challenge                    |
| ----------------- | ------------------------------- | ------------------------ | -------------------------------- |
| Lattice QCD       | Sign problem at $\mu\neq0$      | Mass gaps, phase diagram | Large qubit count                |
| Hubbard model     | Sign problem (frustrated/doped) | Mott transition energy   | Trial state overlap              |
| Band structure    | Correlation effects in DFT      | Topological invariants   | $\mathbf{k}$-point scan overhead |
| Nuclear structure | $A>20$ shell model              | Binding energies         | Basis truncation errors          |
| Quantum chemistry | Strong electron correlation     | Ground state energy      | See [[QPE Chemistry]]            |
All targets share same [[QPE]] structure - what differs is the [[Hamiltonian encoding]] & trial state preparation strategy.
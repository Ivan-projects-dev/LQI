#Algorithm #Math #Chemistry #Hardware 
In **quantum chemistry simulation** [[QPE]] estimates ground-state energy of molecule by running time evolution on quantum computer & reading off the phase.

Molecular Hamiltonian $H$ (describing electron-nuclear interactions) has discrete eigenvalues $E_0 < E_1 < \ldots$ (energy levels). Goal is $E_0$, the ground-state energy.

**Key identity**: if $H|\psi_0\rangle = E_0|\psi_0\rangle$, then the time-evolution unitary satisfies:
$$U(\tau)|\psi_0\rangle = e^{-iH\tau}|\psi_0\rangle = e^{-iE_0\tau}|\psi_0\rangle$$
So $|\psi_0\rangle$ is eigenstate of $U(\tau)$ with eigenvalue $e^{-iE_0\tau}$. [[QPE]] extracts the phase $\varphi = E_0\tau / (2\pi)$, from which $E_0 = 2\pi\varphi / \tau$.
**Workflow:**
1. Map molecular Hamiltonian $H$ to qubit Hamiltonian (Jordan-Wigner or Bravyi-Kitaev encoding)
2. Approximate time evolution $e^{-iH\tau}$ via **Trotterization**: $e^{-iH\tau} \approx \prod_k e^{-iH_k\tau/n}$ for $n$ Trotter steps
3. Prepare approximation $|\tilde{\psi}_0\rangle$ of the ground state (e.g., Hartree-Fock state)
4. Run [[QPE]] with $U = e^{-iH\tau}$
**Overlap requirement**: success probability of [[QPE]] returning $E_0 = |\langle\tilde{\psi}_0|\psi_0\rangle|^2$. Good initial guess is critical.

| | [[QPE]] | [[VQE]] |
|---|---|---|
| Hardware | Fault-tolerant (deep circuits) | NISQ (shallow circuits) |
| Accuracy | Systematically improvable | Bounded by ansatz quality |
| State prep | Needs overlap with ground state | Variational over ansatz |
| Scaling | $O(1/\epsilon)$ in circuit depth | Heuristic |
| Status | Future advantage | Current NISQ use |
[[QPE]] gives the definitive quantum chemistry speedup; [[VQE]] is the near-term workaround.
### See also
- [[QPE]]
- [[QPE apps]]
- [[Qubitization]] - exact alternative: walk operator on LCU of $H$, no Trotter error
- [[Ground State Filtering]] - amplifying overlap when Hartree-Fock trial state is poor
- [[Early FTQC]] - running chemistry [[QPE]] before full fault tolerance is available
- [[QPE Simulation]] - [[QPE]] for lattice gauge, Hubbard & nuclear Hamiltonians
### Sources
- [Aspuru-Guzik et al. 2005: Simulated Quantum Computation of Molecular Energies](https://arxiv.org/abs/quant-ph/0604193)
- [Babbush 2018: Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity](https://arxiv.org/abs/1805.03912)
- [Qiskit Nature documentation](https://qiskit-ecosystem.github.io/qiskit-nature/)
- [IBM Quantum Learning: Quantum chemistry](https://learning.quantum.ibm.com/course/quantum-chemistry-with-vqe)

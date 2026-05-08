#Algorithm #Math
[[QPE]] is the core subroutine behind most exponential quantum speedups. Any problem that reduces to estimating the eigenvalue $e^{2\pi i\varphi}$ of unitary $U$ can be solved by [[QPE]].
$$\text{Q}(U, |u\rangle) \longrightarrow \tilde{\varphi} \approx \varphi \quad \text{to } t \text{ bits of precision}$$
where $Q$ = [[QPE]]

| Algorithm | What $U$ is | What $\varphi$ encodes |
|---|---|---|
| [[Shor factoring\|Shor's algorithm]] | Modular mult. $U_a\|x\rangle = \|ax \bmod N\rangle$ | Order $r$ of $a$ |
| [[QPE Chemistry\|Quantum chemistry (Trotter)]] | Time evolution $e^{-iHt}$ | Ground state energy $E_0$ |
| [[Qubitization\|Quantum chemistry (qubitization)]] | Walk operator $W$ on LCU of $H$ | $E_j = \lambda\cos(2\pi\varphi)$ |
| [[QPE Counting\|Quantum counting]] | [[Grover]] iterate $G$ | num of solutions $M$ |
| [[QPE HHL\|HHL algorithm]] | $e^{iAt}$ for [[Matrix]] $A$ | Eigenvalue $\lambda_j$ of $A$ |
| [[QPE Walks\|Quantum walks]] | Szegedy walk operator $W$ | Spectral gap / mixing time |
| [[MNRS Framework\|Walk search (MNRS)]] | Walk on Johnson / Cayley graph | Marked vertex detection |
| [[QPE Simulation\|Quantum simulation]] | $e^{-iHt}$ for physics Hamiltonians | Energy gaps, phase diagrams |
| [[QPE Optimization\|Optimization]] | $e^{-iH_C\tau}$ for Ising / [[QUBO]] | Ground state = optimal value |
**What changes between applications:**
1. **Unitary $U$** - how you construct & implement it (the hard part)
2. **Eigenstate $|u\rangle$** - how you prepare the input state
3. **Post-processing** - how you convert raw phase $\tilde{\varphi}$ into the answer
**What stays the same:**
- Clock register of $t$ [[Qubits]] ($t$ = precision bits)
- [[Hadamard]] layer on clock
- Inverse QFT on clock
- Measurement & classical readout
To estimate $\varphi$ to $\epsilon$ precision with success probability $\geq 1 - \delta$:
$$t = \left\lceil \log_2 \frac{1}{\epsilon} + \log_2\!\left(2 + \frac{1}{2\delta}\right)\right\rceil \text{ clock Qs}$$
Where $Qs$ - [[Qubits]]
Total calls to controlled-$U$: $\sum_{j=0}^{t-1} 2^j = 2^t - 1$. Dominant cost in all applications.
### Sources
- [Kitaev 1995: Quantum measurements & the Abelian Stabilizer Problem](https://arxiv.org/abs/quant-ph/9511026)
- [Nielsen & Chuang, Ch. 5: Quantum Fourier Transform & its Applications](https://www.cambridge.org/core/books/quantum-computation-and-quantum-information/01E10196D0A682A6AEFFEA52D53BE9AE)
- [IBM Quantum Learning: Phase Estimation & Factoring](https://learning.quantum.ibm.com/course/fundamentals-of-quantum-algorithms/phase-estimation-and-factoring)

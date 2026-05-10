#Algorithm #Math
**Ground state filtering** amplifies overlap between trial state $|\psi\rangle$ & true ground state $|\psi_0\rangle$, enabling [[QPE]] to succeed even when $\eta = |\langle\psi_0|\psi\rangle|^2 \ll 1$. Core subroutine for quantum chemistry, optimization, & any setting where preparing good eigenstate is hard.
### Problem setup
Standard [[QPE]] returns ground-state energy $E_0$ with probability exactly $\eta$ per shot. For Hartree-Fock trial state in strongly correlated molecule, $\eta \sim 10^{-2}$–$10^{-3}$ - requiring $\sim 10^2$–$10^3\times$ more shots. Filtering constructs operator $F$ such that:
$$\frac{F|\psi\rangle}{\|F|\psi\rangle\|} \approx |\psi_0\rangle$$
with $\|F|\psi\rangle\|^2 \gg \eta$. Amplified state is fed into [[QPE]], which then succeeds with high probability.

**Key inputs needed**: rough energy estimate $E_*\approx E_0$ & spectral gap $\Delta = E_1 - E_0$. Both obtainable from classical preprocessing ([[VQE]] output or perturbation theory).

**Gaussian filter**: $$F = e^{-(H - E_*)^2 / (2\sigma^2)}$$Centered at $E_*$, suppresses states with $|E - E_*| \gg \sigma$. Sets overlap gain $G \sim \exp((E_1-E_*)^2/(2\sigma^2))$.
Implementation: express via [[Quantum Fourier Transform|Fourier transform]] of Gaussian: $F = \int_{-\infty}^{\infty} \hat{g}(t)\,e^{-iHt}\,dt$, discretized as $O(\sigma^{-1}\log(1/\epsilon))$ time-evolution queries. Good for dense spectra with many close eigenvalues.

**Power-cosine filter (QSP-based)**:
$$F = T_d\!\left(1 - 2\frac{(H - E_*)^2}{\Delta^2}\right)$$
$T_d$ = Chebyshev polynomial of degree $d$, sharpens from $E_0$ to $E_1$ region. Implemented by $O(d)$ queries to controlled-$e^{-iHt}$ via quantum signal processing. **Near-optimal** query complexity: $O(\Delta^{-1}\log(\eta^{-1}))$ total queries for deterministic ground state preparation.

**Krylov-based filter**
Build filter implicitly from Lanczos vectors $\{|\psi\rangle, H|\psi\rangle, H^2|\psi\rangle, \ldots, H^{d-1}|\psi\rangle\}$ computed quantumly.
Classical post-processing selects optimal polynomial $p(H)$ via eigenvalue decomposition of the small Krylov [[Matrix]] $K_{ij} = \langle\psi|H^{i+j}|\psi\rangle$. Low quantum overhead ($d$ time-evolution steps), but requires classical diagonalization of $d\times d$ [[Matrix]]. 

**[[QPE]]-based filter (post-selection)**
[[QPE]] itself acts as energy filter: run [[QPE]], measure clock register, keep shots where result falls near $E_*$. Filtered state ≈ $|\psi_0\rangle$. Success prob per shot = $\eta$; repeat $O(1/\eta)$ times. Simplest to implement; optimal when $\eta$ is not too small.
### Filtered QPE (FQPE)
Combines filtering & [[QPE]] into single algorithm - applies filter inside the phase estimation loop rather than as separate pre-processing step. Reduces total queries vs naive composition by avoiding double overhead. Oct $2025$ unified framework:
1. Run [[QPE]]-based filter to prepare approximate $|\tilde\psi_0\rangle$
2. Use [[QPE]] on $|\tilde\psi_0\rangle$ to extract $E_0$ - now succeeds with high prob
Or: signal-processing-inspired FQPE directly mitigates the unfavorable $O(1/\eta)$ dependence of standard [[QPE]] on initial overlap.

| Filter | Queries to $e^{-iHt}$ | Requires $\Delta$? | Deterministic? |
|---|---|---|---|
| [[QPE]] post-selection | $O(1/(\epsilon\eta))$ | No | No |
| Gaussian | $O(\sigma^{-1}\log(1/\epsilon))$ | No | No |
| Power-cosine (QSP) | $O(\Delta^{-1}\log(1/\eta))$ | Yes | Yes |
| Krylov (degree $d$) | $O(d)$ quantum + classical work | Implicitly | Hybrid |
| FQPE (unified) | $O(\Delta^{-1}\eta^{-1/2})$ | Yes | Yes |
$\Delta$ = spectral gap $E_1 - E_0$. Smaller gap $→$ harder to filter. When $\Delta$ is unknown, adaptive schemes estimate it alongside $E_0$.
### Connection to amplitude amplification
Power-cosine filter is equivalent to **quantum signal processing** applied to the walk operator from [[Qubitization]]. This connection gives unified view:
- [[Qubitization]] block-encodes $H/\lambda$
- QSP applies polynomial $p(H/\lambda)$ to implement filter $F$
- Cost scales as $O(\deg(p)\cdot\text{cost}(W))$ where $W$ is the walk operator
Optimal polynomial degree $d = O(\lambda\Delta^{-1}\log(1/\eta))$ for power-cosine scheme.
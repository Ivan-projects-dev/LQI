#Algorithm #Quantum
[[QPE]] is **subroutine** in most exponentially-fast quantum algorithms. 
**Unifying pattern**: problem reduces to finding eigenphase of carefully constructed unitary $U$.

**Shor's factoring algorithm**
Target: factor int $N$. Reduces to order-finding - find $r$ such that $a^r \equiv 1 \pmod{N}$ for random $a$.

$U_a|x\rangle = |ax \bmod N\rangle$ has eigenstates $|u_s\rangle = \frac{1}{\sqrt{r}}\sum_{k=0}^{r-1}e^{-2\pi i sk/r}|a^k \bmod N\rangle$ with eigenphases $\varphi_s = s/r$

[[QPE]] on $U_a$ returns approximation to $s/r$ → classical continued fractions recovers $r$ → $\gcd(a^{r/2}\pm 1, N)$ gives factors. 

**Qubit count**: $\sim 2n$ control [[Qubits]] for $n$-bit $N$. 
**Total**: $O(n^3)$ gates → **exponential speedup** over best classical $O(e^{n^{1/3}})$ algorithms.

**[[HHL algorithm]]** (linear systems): solves $Ax = b$ for sparse Hermitian $A$. Eigenphases of $U = e^{iAt}$ encode eigenvalues $\lambda_j$ of $A$. [[QPE]] extracts $\lambda_j$ into [[Ancilla]] register. Controlled rotation then applies $\lambda_j^{-1}$ (inversion). Un-[[QPE]] removes eigenphase register. Result: $|x\rangle \propto A^{-1}|b\rangle$. Complexity: $O(s^2 \kappa^2 \log N / \epsilon)$ vs classical $O(Ns\kappa)$.

**Quantum chemistry**: $U = e^{-iHt}$ via Trotterization. [[QPE]] on $U$ with trial ground state $|\psi_0\rangle$ returns ground-state energy $E_0$ to chemical accuracy ($< 1.6 \times 10^{-3}$ hartree). Primary target for fault-tolerant quantum advantage in chemistry.

**[[Quantum counting]]** (Grover + [[QPE]]): Grover operator $G$ has eigenvalues $e^{\pm 2i\theta}$ where $\sin\theta = \sqrt{M/N}$. [[QPE]] on $G$ returns $\theta$ → $M = N\sin^2\theta$.

**[[Quantum simulation]]**: simulating time evolution $e^{-iHt}$ of physical Hamiltonian: [[QPE]] extracts energy eigenvalues, enabling lattice gauge theory simulations, condensed matter band structure calcs, & quantum phase diagram mapping. [[QPE]] precision $\epsilon$ translates to energy resolution $\epsilon / t$.
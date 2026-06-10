#Math #Algorithm #Chemistry
**Trotterization** (Trotter-Suzuki decomposition) approximates the time-evolution operator $e^{-iHt}$ as a product of simpler exponentials when $H = \sum_k H_k$ is a sum of non-commuting terms.

### The problem
For a [[Hamiltoan|Hamiltonian]] $H = H_1 + H_2$ where $[H_1, H_2] \neq 0$:
$$e^{-i(H_1+H_2)t} \neq e^{-iH_1 t}\,e^{-iH_2 t}$$
Each $e^{-iH_k t}$ may be easy to implement as a gate, but the full $e^{-iHt}$ is not.

### First-order Trotter (Lie-Trotter)
Split evolution time $t$ into $n$ steps of size $\delta t = t/n$:
$$e^{-iHt} \approx \left(e^{-iH_1\delta t}\,e^{-iH_2\delta t}\cdots e^{-iH_m\delta t}\right)^n$$
Error per step: $O(\delta t^2 [H_1, H_2])$. Total error: $O(t^2/n)$ - decreases with more steps.

### Second-order Trotter (Suzuki)
Symmetric split reduces error:
$$e^{-iHt} \approx \left(e^{-iH_1\delta t/2}\,e^{-iH_2\delta t}\,e^{-iH_1\delta t/2}\right)^n$$
Error: $O(\delta t^3)$ per step, $O(t^3/n^2)$ total - much better for same gate count.

Higher-order Suzuki formulas ($p=4,6,\ldots$) exist, trading circuit depth for lower error.

### Gate count
For $m$ Hamiltonian terms and $n$ Trotter steps:

| Order | Gates per step | Total gates | Error |
|-------|---------------|-------------|-------|
| 1st order | $m$ | $mn$ | $O(t^2/n)$ |
| 2nd order | $2m-1$ | $(2m-1)n$ | $O(t^3/n^2)$ |
| $p$-th order | $O(5^p m)$ | $O(5^p m n)$ | $O(t^{p+1}/n^p)$ |

### Use in QPE
[[QPE Chemistry]] and [[QPE Simulation]] need controlled-$U^k$ where $U = e^{-iH\tau}$. Trotterization builds this:
1. Decompose $H = \sum_k H_k$ (e.g., Pauli strings from [[Jordan-Wigner encoding]])
2. Each $e^{-iH_k\delta t}$ is a simple rotation gate (RZ, RZZ, etc.)
3. Repeat $n$ steps × $k$ applications for $U^k$

Total gate cost for [[QPE]] with $t$-bit precision: $O(2^t \cdot mn)$ gates (1st order).

### Trotter error in chemistry
For molecular Hamiltonians with $\eta$ electrons and $N$ spin-orbitals: $m = O(N^4)$ Pauli terms (Jordan-Wigner). Achieving chemical accuracy ($\epsilon < 1.6 \times 10^{-3}$ hartree) requires:
$$n = O\!\left(\frac{(N^4 \Lambda t)^2}{\epsilon}\right) \quad \text{Trotter steps}$$
where $\Lambda$ is the spectral norm of $H$. This is the dominant cost driver in [[QPE Chemistry]].

### Alternatives to Trotterization
[[Qubitization]] and [[LCU]] (Linear Combination of Unitaries) avoid Trotterization entirely, achieving $O(\log(1/\epsilon))$ scaling vs $O(1/\epsilon)$ for Trotter - but with higher constant overhead.

See [[Trotter formula]] for the mathematical derivation. See [[Hamiltoan|Hamiltonian]] for operator structure.

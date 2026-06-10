#Algorithm #Physics #Math
**Hamiltonian simulation** is the problem of implementing the time-evolution unitary $e^{-iHt}$ on a quantum computer given a description of [[Hamiltoan|Hamiltonian]] $H$. It is the central subroutine of [[QPE Chemistry]], [[QPE Simulation]], and quantum dynamics.

### Problem statement
Given: $H = \sum_k \alpha_k H_k$ (sum of local/Pauli terms), target time $t$, error $\epsilon$
Goal: implement unitary $\tilde{U}$ such that $\|\tilde{U} - e^{-iHt}\| \leq \epsilon$

### Methods

#### 1. Trotterization (product formulas)
See [[Trotterization]] for details.
$$e^{-iHt} \approx \left(e^{-iH_1\delta t}\cdots e^{-iH_m\delta t}\right)^n \quad \delta t = t/n$$

**Complexity** (1st order, chemistry): $O\!\left(\frac{(m\Lambda t)^2}{\epsilon}\right)$ gates, $\Lambda = \max_k \|H_k\|$

**Pros**: no [[Ancilla]], simple, hardware-friendly, commutator scaling can be better than worst case
**Cons**: $O(1/\epsilon)$ depth scaling, accumulates errors multiplicatively

#### 2. LCU (Linear Combination of Unitaries)
See [[LCU]] and [[Block encoding]].
$$H = \sum_k \alpha_k U_k \quad \Rightarrow \quad e^{-iHt} \approx \text{PREP}^\dagger \cdot \text{SEL} \cdot \text{PREP} \text{ (repeated)}$$

Uses Jacobi-Anger expansion: $e^{-i\lambda x t} = \sum_k (-i)^k J_k(\lambda t) T_k(x)$ (Chebyshev).

**Complexity**: $O\!\left(\frac{\lambda t}{\epsilon} \cdot \text{polylog}\right)$ where $\lambda = \sum_k |\alpha_k|$

**Pros**: $O(\log(1/\epsilon))$ query scaling via [[Quantum signal processing]]
**Cons**: requires [[Ancilla]] [[Qubits]], PREP/SEL overhead, $\lambda$ can be large

#### 3. Qubitization
See [[Qubitization]].
Build walk operator $W$ from block-encoded $H/\lambda$. The eigenphases of $W$ are $\pm\arccos(E_k/\lambda)$. Apply [[Quantum signal processing]] to $W$ to simulate $e^{-iHt}$.

**Complexity**: $O\!\left(\frac{\lambda t + \log(1/\epsilon)}{\log(\lambda t/\epsilon)}\right) \approx O(\lambda t)$ (near-optimal)

**Pros**: optimal gate scaling, exact to any precision
**Cons**: highest implementation complexity, requires [[Block encoding]] infrastructure

#### 4. QDRIFT (randomized)
Sample Hamiltonian terms $H_k$ randomly with probability $\propto \alpha_k$, apply $e^{-iH_k\tau}$ with $\tau = \lambda t/n$:

**Complexity**: $O\!\left(\frac{\lambda^2 t^2}{\epsilon}\right)$ - worse asymptotically, but no commutator overhead

**Pros**: simple, no PREP/SEL, error is random (variance) not systematic (bias)
**Cons**: needs more repetitions; variance grows with system size

### Complexity comparison

| Method | Gate count (dominant) | [[Ancilla]] | Error model | Best for |
|--------|----------------------|---------|-------------|----------|
| Trotter 1st | $O(m\Lambda^2 t^2/\epsilon)$ | None | Systematic | Small circuits, NISQ |
| Trotter 2nd | $O(m\Lambda^{3/2} t^{3/2}/\epsilon^{1/2})$ | None | Systematic | Medium depth |
| [[LCU]] + QSP | $O(\lambda t \log(1/\epsilon))$ | $O(\log m)$ | Random | [[FTQC]], large $N$ |
| [[Qubitization]] | $O(\lambda t)$ (optimal) | $O(\log m)$ | Exact | [[FTQC]], best asymptotic |
| QDRIFT | $O(\lambda^2 t^2/\epsilon)$ | None | Random | Shallow, stochastic |

$m$ = number of Hamiltonian terms, $\Lambda$ = largest term norm, $\lambda$ = 1-norm of coefficients.

### Tradeoff landscape
- **Few [[Ancilla]] [[Qubits]]** → use Trotter
- **Best asymptotic cost** → [[Qubitization]] / QSP
- **Randomized error preferred** → QDRIFT or randomized Trotter
- **Hybrid** (early-[[FTQC]]) → randomized Trotter + single-[[Ancilla]] [[QPE]]

### Practical note: which wins for chemistry?
For molecules at chemical accuracy ($\epsilon \sim 10^{-3}$ hartree), crossing point where [[Qubitization]] beats Trotter occurs around $N \sim 50$–100 spin-orbitals. Below this, optimized Trotter (with commutator bounds) is often cheaper in total T-gate count despite worse asymptotic scaling.

See [[QPE Chemistry]] for end-to-end workflow. See [[Trotterization]], [[LCU]], [[Qubitization]] for individual method details.

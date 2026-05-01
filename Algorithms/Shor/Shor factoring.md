#Algorithm #Cybersecurity #Math
Factoring $N$ (finding $p, q$ such that $N = pq$) is hard. But [[Shor]] showed that factoring reduces to **order-finding**, which quantum computers can do efficiently.

**Order-finding:** given $N$ & a randomly chosen $a$ with $\gcd(a, N) = 1$, find the smallest positive int $r$ such that:
$$a^r \equiv 1 \pmod{N}$$
This $r$ is called the **multiplicative order** of $a$ modulo $N$.

Once you have $r$, the factors fall out of classical number theory:
- If $r$ is even & $a^{r/2} \not\equiv -1 \pmod{N}$, then
$$\gcd(a^{r/2} - 1,\, N) \quad \text{and} \quad \gcd(a^{r/2} + 1,\, N)$$
are non-trivial factors of $N$ with high probability.

**Why this works:** $a^r - 1 \equiv 0 \pmod{N}$, so $N$ divides $(a^{r/2}-1)(a^{r/2}+1)$. If neither factor is $\equiv 0 \pmod N$, then $N$ shares a factor with each.

---
## Order-Finding via QPE
Classical order-finding is exponentially hard - there is no efficient algorithm. The quantum speedup comes from applying [[QPE]] to a carefully chosen unitary.

**Define the modular exponentiation unitary:**
$$U_a |x\rangle = |ax \bmod N\rangle$$
This unitary applies multiplication by $a$ modulo $N$. Its eigenstates are:
$$|u_s\rangle = \frac{1}{\sqrt{r}} \sum_{k=0}^{r-1} e^{-2\pi i sk/r} |a^k \bmod N\rangle, \quad s = 0, 1, \ldots, r-1$$
with eigenvalues $e^{2\pi i s/r}$. The eigenphases are $\varphi_s = s/r$.

[[QPE]] applied to $U_a$ with an eigenstate input returns $s/r$ in binary to the phase register.

---

**Step 1 - Classical preprocessing:**
- Pick a random $a \in \{2, \ldots, N-1\}$
- Compute $\gcd(a, N)$. If $> 1$, you already found a factor (rare but free)
- Proceed to quantum order-finding

**Step 2 - Quantum: prepare superposition**

Initialize $t$ control qubits (phase register) in $|0\rangle$ & $n$ target qubits in $|1\rangle$ (representing $x = 1$):
$$|0\rangle^{\otimes t} |1\rangle$$
Apply Hadamard to all control qubits:
$$\frac{1}{\sqrt{2^t}} \sum_{j=0}^{2^t - 1} |j\rangle |1\rangle$$
**Step 3 - Quantum: controlled modular exponentiation**

Apply $U_a^j$ controlled on $|j\rangle$:
$$\frac{1}{\sqrt{2^t}} \sum_{j=0}^{2^t - 1} |j\rangle |a^j \bmod N\rangle$$
This is the computationally expensive quantum step. Building an efficient circuit for modular exponentiation is the main engineering challenge in implementing Shor's algorithm.

**Step 4 - Quantum: Inverse QFT on phase register**

Apply the [[Quantum Fourier Transform|QFT]]$^\dagger$ to the control register. This transforms the periodic pattern of $|a^j \bmod N\rangle$ into a peak at $s/r$ in the frequency domain.

Measurement gives an approximation $\tilde{s}/\tilde{r}$ close to $s/r$.

**Step 5 - Classical: continued fractions**

Use the continued fraction algorithm to extract $r$ from the approximation $s/r$. The convergents of the continued fraction expansion of $\tilde{s}/2^t$ give candidates for $r$.

**Step 6 - Classical: extract factors**

With $r$ in hand:
$$p = \gcd(a^{r/2} - 1, N), \quad q = \gcd(a^{r/2} + 1, N)$$
Verify: $p \times q = N$. If this fails (e.g., $r$ is odd, or $a^{r/2} \equiv -1$), pick a new $a$ & repeat. Success probability $> 50\%$ per attempt.

| Resource                      | Cost                   |
| ----------------------------- | ---------------------- |
| Phase register [[Qubits]] $t$ | $2n$ (for $n$-bit $N$) |
| Target register [[Qubits]]    | $n$                    |
| Total logical [[Qubits]]      | $\sim 3n$              |
| Gate count                    | $O(n^3)$               |
| Classical post-processing     | $O(n^2)$               |
For [[RSA]]-2048: $n = 2048$ bits $→$ $\sim 6{,}000$ logical [[Qubits]], $O(2048^3) \approx 8.6 \times 10^9$ gates.
## Sources
- [Shor 1994 original paper](https://arxiv.org/abs/quant-ph/9508027)
- [Nielsen & Chuang, Ch. 5: Quantum Fourier Transform & its Applications](https://www.cambridge.org/core/books/quantum-computation-and-quantum-information/01E10196D0A682A6AEFFEA52D53BE9AE)
- [IBM Quantum Learning: Phase Estimation & Factoring](https://learning.quantum.ibm.com/course/fundamentals-of-quantum-algorithms/phase-estimation-and-factoring)
- [Qiskit Textbook: Shor's Algorithm](https://github.com/Qiskit/textbook)

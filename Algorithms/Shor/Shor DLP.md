#Algorithm #Cybersecurity #Math #Q-Sharp
**Discrete [[Logarithm]] Problem (DLP):** Given cyclic group $G = \langle g \rangle$ of order $r$, generator $g$, & element $h \in G$ with $h = g^x$, find the exponent $x \in \{0, 1, \ldots, r-1\}$.

Classical hardness: no polynomial-time algorithm is known for general DLP. Best classical algorithms run in sub-exponential to $sqrt$ time depending on the group.

**[[Shor]]'s DLP algorithm** solves this in $O((\log r)^3)$ time - exponential speedup.

[[Shor]]'s factoring algorithm finds the period $r$ of $f(x) = a^x \bmod N$. DLP is also period-finding problem, but over **$2$-dimensional lattice** instead of $\mathbb{Z}$.

Define the func: $$f(a, b) = g^a \cdot h^{-b} = g^{a - xb} \in G$$
This func is **doubly periodic**: it satisfies
$$f(a + r,\, b) = f(a, b) \qquad \text{and} \qquad f(a + x,\, b + 1) = f(a, b)$$
Lattice of periods is $\Lambda = \{(kr, 0) + m(x, 1) : k, m \in \mathbb{Z}\}$.

[[Quantum Fourier Transform]] over $\mathbb{Z}_r \times \mathbb{Z}_r$ extracts the **dual lattice** of $\Lambda$, from which $x$ is recovered classically.

Work modulo $r$ (or with $N = 2^n \geq r$ for power-of-two QFT). Use $2$ $n$-qubit registers for $a$ & $b$.
### **1. Superposition:**
$$|0\rangle|0\rangle \xrightarrow{H^{\otimes n} \otimes H^{\otimes n}} \frac{1}{N}\sum_{a=0}^{N-1}\sum_{b=0}^{N-1} |a\rangle|b\rangle$$
### **2. [[Oracle]] evaluation** (compute $f(a,b) = g^a h^{-b}$ into [[Ancilla]]):
$$\frac{1}{N}\sum_{a,b} |a\rangle|b\rangle|g^a h^{-b}\rangle$$
### **3. Measure [[Ancilla]]:** collapses to uniform superposition over all $(a, b)$ pairs consistent with $1$ specific group element $c = g^{a_0} h^{-b_0}$:
$$\frac{1}{\sqrt{r}} \sum_{k=0}^{r-1} |a_0 + kx\rangle|b_0 + k\rangle \quad (\text{modulo }r)$$
### **4. $2D$ [[Quantum Fourier Transform]]** on both registers:
$$\text{QFT}_N \otimes \text{QFT}_N$$
QFT converts the periodic lattice into peaks at the **reciprocal lattice** vectors. Measurement gives pair $(f_1, f_2)$ approximately equal to $(k_1 r / N, \, k_2 r / N)$ for random integers $k_1, k_2$ with $\gcd(k_1, k_2, r) = 1$
### **5. Classical post-processing:**
From the measured pair $(f_1, f_2)$, form the ratio:
$$\frac{f_1}{f_2} \approx \frac{k_1}{k_2} \cdot \frac{r/N}{r/N} = \frac{k_1}{k_2}$$
But the key relationship from the dual lattice is that the measurement outcome $(f_1, f_2)$ satisfies:
$$f_1 \cdot 1 - f_2 \cdot x \equiv 0 \pmod{r}$$
So $x \equiv f_1 / f_2 \pmod{r}$. Use the extended Euclidean algorithm to compute $f_2^{-1} \bmod r$ & multiply:
$$x = f_1 \cdot f_2^{-1} \bmod r$$
Repeat with fresh circuit if $\gcd(f_2, r) \neq 1$ (with constant probability this fails; each repeat succeeds with probability $\geq 1/2$).

| Task                                  | Classical                       | [[Shor]]'s quantum |
| ------------------------------------- | ------------------------------- | ------------------ |
| Factoring $n$-bit $N$                 | $O(e^{n^{1/3} (\log n)^{2/3}})$ | $O(n^3)$           |
| DLP in $\mathbb{Z}_p^*$ ($n$-bit $p$) | $O(e^{n^{1/3} (\log n)^{2/3}})$ | $O(n^3)$           |
| DLP in EC group ($n$-bit order)       | $O(2^{n/2})$ (Pollard rho)      | $O(n^3)$           |
For DLP in group of order $r$ ($n = \lceil \log_2 r \rceil$ bits):
- $2$ QPE registers: $2n$ qubits each $→$ $4n$ control qubits total
- Group element ancilla register: $n$ qubits for the group element
- **Total logical qubits:** $\sim 5n$
- For ECDH P-256 ($n = 256$): $\sim 1{,}280$ logical qubits
- Physical qubits (surface code, $p = 10^{-3}$): $\sim 2{,}000{,}000$

This is roughly $1/2$ the qubit count of $RSA-2048$ factoring - **ECDH 256 is harder to break classically but easier to break quantumly** than $RSA-2048$ as group operation circuit (elliptic curve point addition) is somewhat simpler than modular exponentiation at the same security level.

| Feature                  | Factoring                           | DLP                                          |
| ------------------------ | ----------------------------------- | -------------------------------------------- |
| func                     | $f(x) = a^x \bmod N$                | $f(a, b) = g^a h^{-b} \in G$                 |
| Periods to find          | $1$ period $r$                      | Lattice in $\mathbb{Z}^2$                    |
| [[QPE]] registers        | $1$ register ($\sim 2n$ [[Qubits]]) | $2$ registers ($\sim 2n$ each)               |
| QFT                      | $1$D QFT                            | $2$D QFT ($= \text{QFT} \otimes \text{QFT}$) |
| Post-processing          | Continued fractions                 | Modular inverse                              |
| [[Oracle]] calls per run | $O(n)$ (controlled $U^{2^k}$)       | $O(n)$ per register ($2 \times O(n)$ total)  |
## Sources 
- [Shor 1994: Algorithms for quantum computation - discrete logarithms & factoring](https://arxiv.org/abs/quant-ph/9508027)
- [Preskill: Quantum Computing lecture notes Ch. 6 (period finding, DLP)](http://theory.caltech.edu/~preskill/ph219/chap6_19.pdf)
- [Ekerå & Håstad 2017: Quantum algorithms for computing short discrete logarithms](https://arxiv.org/abs/1703.09077)
- [Beauregard 2002: Circuit for Shor's algorithm using 2n+3 qubits](https://arxiv.org/abs/quant-ph/0205095)
- [Crypto.SE: Using Shor's algorithm for discrete logarithm](https://crypto.stackexchange.com/questions/37018)

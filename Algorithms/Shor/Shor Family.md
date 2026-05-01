#Algorithm #Cybersecurity #Math 
All algorithms in this family share common structure:
1. Encode the target problem as func $f: G \to X$ that hides subgroup or period
2. Use quantum [[Oracle]] to evaluate $f$ in superposition
3. Apply the [[Quantum Fourier Transform]] (or variant) over the group $G$
4. Measure & recover the hidden structure classically

They differ in which group $G$ they use, what $f$ encodes, & what post-processing extracts the answer.

---

| Algorithm        | Problem                       | Group            | QFT            | Speedup          | Breaks               |
| ---------------- | ----------------------------- | ---------------- | -------------- | ---------------- | -------------------- |
| [[Shor]] factoring   | $N = pq$                      | $\mathbb{Z}_N^*$ | $1D$           | Exponential      | [[RSA]]                  |
| [[Shor]] DLP         | $h = g^x$ in $\mathbb{Z}_p^*$ | $\mathbb{Z}^2$   | $2D$           | Exponential      | DH, DSA              |
| [[Shor]] ECDLP       | $Q = kP$ on EC                | EC group         | $2D$           | Exponential      | [[ECDH]], ECDSA          |
| [[Simon]]'s          | $f(x) = f(x \oplus s)$        | $\mathbb{Z}_2^n$ | Walsh-[[Hadamard]] | Exponential      | (theoretical)        |
| [[Kitaev iterative]] | Order/phase finding           | -                | Semi-classical | Exponential      | [[RSA]] (fewer [[Qubits]])   |
| [[Ekerå-Håstad]]     | Short DLP                     | $\mathbb{Z}^2$   | $2D$ (reduced) | Exponential      | DH with short keys   |
| [[Regev 2023]]       | Factoring                     | $\mathbb{Z}_N^*$ | Multi-dim      | Poly improvement | [[RSA]] (gate savings)   |
| [[Hallgren 2002]]    | Principal ideal               | $\mathbb{R}$     | Continuous     | Exponential      | Some lattice schemes |
## Sources
- [Shor 1994: Original paper covering both factoring & DLP](https://arxiv.org/abs/quant-ph/9508027)
- [Ekerå & Håstad 2017: Quantum algorithms for computing short discrete logarithms](https://arxiv.org/abs/1703.09077)
- [Roetteler, Naehrig, Svore, Lauter 2017: Quantum resource estimates for ECDLP](https://arxiv.org/abs/1706.06752)
- [Regev 2023: An efficient quantum factoring algorithm](https://arxiv.org/abs/2308.06572)
- [Hallgren 2002: Polynomial-time quantum algorithms for Pell's equation & the principal ideal problem](https://arxiv.org/abs/quant-ph/0206176)
- [Griffiths & Niu 1996: Semiclassical Fourier transform for quantum computation](https://arxiv.org/abs/quant-ph/9511007)
- [Childs & van Dam 2010: Quantum algorithms for algebraic problems (survey)](https://arxiv.org/abs/0812.0380)

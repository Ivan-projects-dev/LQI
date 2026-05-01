#Cybersecurity #Algorithm 
In $1994$ Peter Shor devised quantum algorithm for factoring large integers, demonstrating the potential of quantum computers to break classical cryptographic systems. The same technique extends to discrete logarithms & unifies an entire family of exponentially fast quantum algorithms.

Classical computers cannot factor large numbers efficiently. The best known classical algorithm (General Number Field Sieve) runs in sub-exponential time $O(e^{n^{1/3}})$ for an $n$-bit number. Shor's algorithm runs in polynomial time $O(n^3)$ - an exponential speedup.

### Core Algorithm
- [[Shor factoring]] - order-finding, [[QPE]], continued fractions
- [[Shor Targets]] - [[RSA]], ECC, DH, & what breaks when
- [[Shor Practical Example]] - running small Shor instances in Qiskit (N=15, N=21)
- [[Shor Timeline & Scale]] - what hardware is needed to break [[RSA]]-2048
- [[Post-Quantum Cryptography]] - NIST [[PQC]] standards, what replaces broken systems

### Extensions
- [[Shor DLP]] - 2D [[QPE]] for DLP in Z_p* & Z_q, with Q# circuit sketch
- [[Hidden Subgroup Problem]] - the unifying framework: factoring, DLP, [[Simon]]'s, & graph isomorphism as special cases
- [[Shor Family]] - ECDLP (elliptic curves), [[Kitaev iterative]] [[QPE]], [[Ekerå-Håstad]] short-key variant, [[Regev 2023]] improvement
## Sources
- [Shor 1994 original paper](https://arxiv.org/abs/quant-ph/9508027)
- [IBM Quantum Learning: Phase Estimation & Factoring](https://learning.quantum.ibm.com/course/fundamentals-of-quantum-algorithms/phase-estimation-and-factoring)
- [Childs & van Dam: Quantum algorithms for algebraic problems (survey)](https://arxiv.org/abs/0812.0380)

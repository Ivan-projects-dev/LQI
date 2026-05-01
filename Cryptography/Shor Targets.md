#Algorithm #Cybersecurity #Cryptography
Which cryptographic systems [[Shor]]'s algorithm breaks, which it does not, & what the transition looks like.

[[Shor]]'s algorithm breaks cryptography whose security depends on two specific math problems:
1. **Int factorization** - given $N = pq$, find $p$ & $q$
2. **Discrete [[Logarithm]]** - given $g^x \bmod p$, find $x$
3. **Elliptic curve discrete [[Logarithm]]** - given $kP$ on an elliptic curve, find $k$
Anything built on these $3$ problems is broken by sufficiently large fault-tolerant quantum computer.

Systems That Break: [[RSA]], [[ECDH]]
### Systems That Do NOT Break
1. **Symmetric Encryption (AES, ChaCha20)**: [[Shor]]'s algorithm has no known application to symmetric key ciphers. Relevant quantum threat to symmetric encryption is [[Grover]]'s Algorithm, which provides quadratic speedup for brute-force search.
2. **Hash funcs (SHA-256, SHA-3)**: [[Grover]] applies to preimage attacks, giving a quadratic speedup. 
SHA-256 drops from $128$-bit collision resistance to $\sim 85$ bits. **Mitigation: use SHA-384 or SHA-512.**
3. **Lattice-Based Cryptography**: Cryptographic systems based on the hardness of lattice problems (Learning With Errors, Ring-LWE, Module-LWE) have no known polynomial-time quantum algorithm. These are the basis for NIST's post-quantum standards.
### What Shor's Algorithm Does Not Affect (Practically, Today)
- Bitcoin addresses (**ECDSA**) - **only vulnerable if public key is exposed before a transaction is mined**. Reusing addresses is dangerous; single-use addresses have a safety window.
- Symmetric session data - AES-256 encrypted data is safe with key doubling.
- Passwords stored as bcrypt/Argon2 hashes - these rely on computational cost, not algebraic hardness.

| Milestone                                                | Estimated year | Confidence                 |
| -------------------------------------------------------- | -------------- | -------------------------- |
| Factor $N = 2048$-bit [[RSA]]                                | $2030$–$2040$  | Moderate                   |
| First CRQC (Cryptographically Relevant Quantum Computer) | $2030$–$2050$  | Low - high uncertainty     |
| Post-quantum TLS deployed globally                       | $2027$–$2030$  | High - already in progress |
| NIST [[PQC]] standards finalized                         | $2024$ (done)  | Confirmed                  |
See also - [[Data harvest]]
## Sources
- [NIST Post-Quantum Cryptography standardization](https://csrc.nist.gov/projects/post-quantum-cryptography)
- [NSA: Commercial National Security Algorithm Suite 2.0](https://media.defense.gov/2022/Sep/07/2003071834/-1/-1/0/CSA_CNSA_2.0_ALGORITHMS_.PDF)
- [Azure Quantum Resource Estimator: RSA factoring](https://learn.microsoft.com/en-us/azure/quantum/overview-resource-estimator)
- [Webber et al. 2022: The impact of hardware specifications on reaching quantum advantage in factoring](https://arxiv.org/abs/2203.13816)
- [NIST IR 8547: Transition to Post-Quantum Cryptography Standards](https://doi.org/10.6028/NIST.IR.8547.ipd)

#Cryptography #Math
**Lattice-based cryptography** is the dominant family of **post-quantum cryptographic** algorithms — resistant to both classical and quantum attacks, including [[Shor]]'s algorithm. Selected by NIST in 2024 as the primary PQC standard.

### Why lattices are quantum-resistant
Shor's algorithm exploits the **hidden subgroup structure** of RSA (integer factoring) and [[ECDH]] (discrete log). Lattice problems — particularly **Learning With Errors (LWE)** and **Module LWE** — have no known quantum speedup beyond Grover's $\sqrt{}$ factor. The best quantum algorithms for lattice problems still require exponential time.

### Core hard problem: LWE
**Learning With Errors**: given many samples $(a_i, b_i)$ where $b_i = \langle a_i, s \rangle + e_i \pmod{q}$ and $e_i$ is small random noise, recover the secret $s$.

Without the noise, this is trivial linear algebra. The noise makes it exponentially hard even for quantum computers. Security reduces to the hardness of **approximating shortest vector** in a high-dimensional lattice (SVP/CVP).

### NIST PQC standards (2024)
| Standard | Problem | Use case |
|---|---|---|
| **ML-KEM** (Kyber) | Module LWE | Key encapsulation (replaces ECDH/RSA key exchange) |
| **ML-DSA** (Dilithium) | Module LWE + SIS | Digital signatures (replaces ECDSA/RSA signatures) |
| **SLH-DSA** (SPHINCS+) | Hash functions | Stateless hash-based signatures (conservative backup) |

ML-KEM and ML-DSA are lattice-based. SLH-DSA is hash-based (different security assumption, larger signatures).

### Why it matters for quantum computing research
- **"Harvest now, decrypt later"** attacks: adversaries capture encrypted data today, intending to decrypt with quantum computers in future. Long-lived secrets (health records, government data) need migration to PQC now.
- **Cryptographically relevant quantum computers (CRQCs)**: estimated 2030s–2040s for breaking RSA-2048. Migration timelines for critical infrastructure are already starting.
- [[Shor Targets]] lists which specific key sizes and algorithms are at risk.

### Migration status (2025)
- US federal agencies: NIST mandates PQC migration by 2030
- TLS 1.3: hybrid classical+PQC key exchange in testing (X25519Kyber768)
- Signal protocol: already upgraded to post-quantum key exchange

Source: [NIST PQC standards — NIST](https://csrc.nist.gov/projects/post-quantum-cryptography) [CRYSTALS-Kyber specification](https://pq-crystals.org/kyber/)

#Cybersecurity Can be broken by [[Shor]]
**Used for:** HTTPS/TLS handshakes, SSH key exchange, email encryption (PGP/S-MIME), code signing, VPN authentication, document signing.

**Why it breaks:** RSA security rests entirely on the difficulty of factoring the public modulus $N = pq$. [[Shor]]'s algorithm factors $N$ in polynomial time.

| RSA key size | Classical hardness | Logical [[Qubits]] needed | Physical [[Qubits]] (est.) |
| ------------ | ------------------ | ------------------------- | -------------------------- |
| RSA-$1024$   | Deprecated by NIST | $\sim 3{,}000$            | $\sim 3{,}000{,}000$       |
| RSA-$2048$   | Current standard   | $\sim 6{,}000$            | $\sim 4{,}000{,}000$       |
| RSA-$4096$   | High security      | $\sim 12{,}000$           | $\sim 8{,}000{,}000$       |

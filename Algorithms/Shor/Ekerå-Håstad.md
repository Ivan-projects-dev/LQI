#Algorithm #Math 
**Target:** DLP when the exponent $x$ is known to be short - $x < 2^d$ where $d \ll n = \log_2 r$.

Many cryptographic protocols use short exponents (e.g., small private keys for performance). Short DLP is easier to break quantumly.

Instead of full $n$-qubit second register (for $b$), use only $d + s$ [[Qubits]] where $s$ is small security margin. QFT operates on smaller domain, reducing total qubit count.

**Resource reduction for [[ECDH]] P-256 (d ≈ 256):**

| Method                  | Logical [[Qubits]]                                                |
| ----------------------- | ------------------------------------------------------------- |
| Standard [[Shor]] DLP       | $\sim 4n = 1{,}024$                                           |
| Ekerå-Håstad            | $\sim 2n + 2d \approx 1{,}024$ (similar for full-length keys) |
| Ekerå $2021$ (combined) | $\sim 2n$ (improved circuit)                                  |
Main benefit appears when $d \ll n$ - e.g., for DSA with $160$-bit keys in $1024$-bit group, the qubit count drops significantly.
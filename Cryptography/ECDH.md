#Cybersecurity #Algorithm **Elliptic Curve Diffie-Hellman**
Can be broken by [[Shor]]

**Used for:** TLS/HTTPS key exchange (the majority of internet traffic), Signal Protocol, WireGuard VPN, most modern secure communications.

**Why it breaks:** ECDH relies on elliptic curve discrete [[Logarithm]]. Both are solved by variants of [[Shor]]'s algorithm.

**ECDH is $>$ compact but equally vulnerable:** a $256$-bit elliptic curve key (equivalent to [[RSA]]-3072 classical security) requires $<$ logical [[Qubits]] than [[RSA]]-2048 to break, not more. Smaller key = simpler group structure for the quantum algorithm.

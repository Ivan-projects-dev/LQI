#Quantum #Math Regev's 2023 Factoring Algorithm

Replaces [[Shor]]'s single modular exponentiation (cost $O(n^3)$ gates) with a multi-dimensional lattice approach using $O(\sqrt{n})$ modular multiplications, each over a smaller modulus.

**Gate complexity:** $O(n^{3/2})$ - polynomial improvement over [[Shor]]'s $O(n^3)$.

**Qubit count:** Requires $O(n^{3/2})$ [[Qubits]] (more [[Qubits]] than [[Shor]]'s $O(n)$ due to the multi-dimensional construction).

**Status:** Theoretical improvement. For practical [[RSA]]-2048 ($n = 2048$), the constant factors mean Regev's algorithm is not yet clearly better than optimized [[Shor]] variants in terms of physical resources.
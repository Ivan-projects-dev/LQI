#Algorithm #Math **Quantum Algorithm for Principal Ideal Problem**

Given ideal $I$ in the ring of ints of num field $\mathbb{Q}(\sqrt{D})$, determine if $I$ is principal & find generator.

Relevant to the security of certain **ideal-lattice** cryptosystems whose hardness reduces to the principal ideal problem (e.g., some NTRU/NTRU Prime variants exploiting ring structure). Does **not** threaten LWE, Ring-LWE, or Module-LWE based schemes - the foundation of NIST [[PQC]] standards (CRYSTALS-Kyber, CRYSTALS-Dilithium). Hallgren showed a polynomial-time quantum algorithm via HSP over $\mathbb{R}$ (continuous group, handled with careful discretization).

$1st$ quantum algorithm for num-theoretic problem over continuous/infinite group. Extended the HSP framework beyond finite groups.


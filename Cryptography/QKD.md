#Cybersecurity #Algorithm #Networks 
**Quantum key distribution (QKD)** ($BB84$ protocol) is secure communication tech that uses quantum mechanical concepts to construct cryptographic protocol. It allows $2$ parties to generate shared, randomly generated secret key that is only known to them. With this key, communications may be encrypted & decrypted. Distributes symmetric keys with info-theoretic security - not computational hardness. It is already deployed in some critical infrastructure (China's quantum backbone network, some European financial networks).
- **Entanglement-based protocols:** $2$ items are coupled to generate single [[Quantum state]]. According to entanglement, measuring $1$ entity influences the other. Other parties involved will be aware if eavesdropper gains access to previously trusted node & makes any modifications.
- **Prepare-and-measure protocols:** goal - measure unidentified quantum states. They may be used to identify possible data interceptions as well as instances of eavesdropping.

Process involves the following steps:
- Several light particles, or photons, are sent between parties via fiber optic cables in order for QKD to func.
- Photons are transmitted in stream of $1$s & $0s$, with each photon having random [[Quantum state]].
- Photon passes via beam splitter on its way to the receiving end, which compels it to choose random path into photon collector.
- Sender receives info from receiver about the photon sequence that was transmitted, which they compare with the photon sequence that the emitter would have sent.
- Certain bit sequence is all that remains after photons in the incorrect beam collector are eliminated.
- During error repair phase & other post-processing procedures, any mistakes & data leaks are eliminated.

**QKD Attack Methods**:
- QKD appears secure in principle, security might be jeopardized by shoddy QKD implementations. Real-world apps have shown methods for breaking into QKD systems.
- Eavesdropper Backdoor was intended to be created via the phase remapping exploit. The attack exploits the requirement for $1$ party member to permit signals to enter & depart their device.
- Photon num splitting attack is additional attack tech. Perfect scenario would allow $1$  user to communicate with another user by sending $1$ photon at a time.
- **Coherent (joint) attacks** are the most general attack class: the eavesdropper stores probes entangled across multiple transmitted [[Qubits]] & measures them collectively after the full transmission. Security proofs that hold against coherent attacks (e.g., [[Shor]]-Preskill, Renner entropic security) provide the strongest composable guarantees & form the basis of modern QKD security analysis.

**Limitation:** requires quantum hardware end-to-end (quantum repeaters for long distances), which is expensive & not scalable to the internet. 
**[[PQC]]** is soft that runs on classical hardware. Both approaches exist in parallel - QKD for highest-security point-to-point links, [[PQC]] for general internet cryptography.
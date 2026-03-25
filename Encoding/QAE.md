#Physics #Quantum #ML 
**Quantum [[Autoencoder]] (QAE)** aims to **compress** an $n$-qubit state into fewer [[Qubits]] by leveraging a parametrized quantum circuit. Suppose we have:
- A register $A$ of $n$ [[Qubits]] & another register $B$ of $k$ [[Qubits]] (to store the compressed % or "trash state").
- A training set ${|ψi⟩AB}$ of pure states, each occupying $n+k$ [[Qubits]] in total.
We define a **parametrized unitary** $U(θ)$ that encodes the input state into a subspace of size n [[Qubits]], discarding the rest.

Then, an **inverse/adjoint** circuit $U†(θ)$ _decodes_ it. The training seeks to _maximize_ the fidelity between the original state & the reconstructed state, or equivalently, to **keep** a reference (auxiliary) state $|φ⟩B′$ unaltered after the encoding-decoding cycle. Formally: $$L(θ)=−∑i⟨ψi|U†(θ)⊗I[U(θ)⊗I]|ψi⟩$$
Minimizing (or maximizing negative) $L(θ)$ yields an optimal circuit $U∗(θ)$ that compresses each $|ψi⟩$ into a smaller subspace.

Let's note that $U∗(θ)$ refers to the **optimal version** of the same **parametrized unitary** $U(θ)$ that the [[Autoencoder]] uses for both _encoding_ and _decoding_. Meaning the params $θ∗$ for which $U(θ∗)$ does the best possible job of compressing (and then decompressing) the quantum data have been found.
![[Pasted image 20260101231618.png]]
QAEs offer a powerful **dimensionality reduction tool** in quantum computing. By cleverly training a unitary circuit to preserve only necessary quantum info, QAEs can reduce resource usage, remove noise, & serve as subroutines for $>$ complex algorithms. While practical deploys face hardware constraints (coherence times, gate fidelity), the approach holds promise for tasks like quantum data compression & robust state preparation in chemistry or advanced quantum ML workflows.
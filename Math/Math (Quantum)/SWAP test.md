#Math #Quantum 
Instead of measuring full fidelity directly, many [[QAE]] implementations use a SWAP test to compare a "trash state" with a fixed reference state $|φ⟩$. By minimizing the measured distance between these two states, the [[QAE]] effectively ensures compression plus faithful reconstruction.

Therefore, the "…" in the middle of the expression for $L(θ)$ could refer to the **SWAP operation** (or whatever **measurement/comparison** operator is being used) between the "trash" [[Qubits]] & the fixed reference state. A common choice is to literally insert $SWAPB, B′$, so the loss might look like:
$$L(θ)=−∑i⟨ψi∣∣(U†(θ)⊗IB′)SWAPB,B′(U(θ)⊗IB′)∣∣ψi⟩$$
Here, the **SWAP gate** (or partial measurement) is used to compare the "trash" % of the state with the auxiliary [[Qubits]] $|φ⟩B′$, ensuring that if the [[Autoencoder]] correctly compresses & decompresses, the auxiliary reference remains unchanged - maximizing fidelity.
![[Pasted image 20260101231954.png]]

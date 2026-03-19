#Quantum #ML 
**QRNN** aims to replicate the RNN's "memory" concept in quantum circuit:
1. **Hidden (History) Register**: subset of [[Qubits]], storing the recurrent hidden state (unmeasured, preserving coherence across time steps).
2. **Input Register**: Another set of [[Qubits]] that encodes each new data point $xt$ (e.g., angle encoding).
3. **Parametrized Quantum Gates**: "quantum recurrent block" that entangles the hidden [[Qubits]] with the newly encoded input, thereby updating the quantum hidden state.
4. **Partial Measurement**: Output [[Qubits]] can be measured to produce classical output $yt$. The hidden [[Qubits]] remain unmeasured to carry the memory forward.

Such block is repeated for each time step $t=1,…,T$. Entire network can be optimized with cost func (e.g., mean-squared error or cross-entropy), & training uses quantum gradient methods (param-shift, finite differences) or classical backprop in simulation env.

**Quantum recurrent block** works:
- The **hidden register** $|ψht−1⟩$ retains memory from the previous time step $t−1$
	- The **input** $|ϕ(xt)⟩$ encodes the new data $xt$ (e.g., via angle encoding)
- A **parametrized unitary** $U(θt)$ entangles these two parts
- Some [[Qubits]] are partially measured to yield the classical output $yt$, while the remainder remain unmeasured to become the next hidden state $|ψht⟩$
This process can be expressed as: $|ψht⟩⊗|yt⟩=U(θt)(|ψht−1⟩⊗|ϕ(xt)⟩)$

where measuring $|yt⟩$ provides the output at step $t$. Diagram of such quantum recurrent block is below, showing "hidden" set of [[Qubits]] passing forward unmeasured, fresh "input" qubit register, set of controlled or entangling gates (the unitary $U(θt)$), & partial measurement for the output. This flow repeats for each time step, carrying quantum memory across the entire sequence.
![[Pasted image 20260101164201.png]]

A straightforward way of building the QRNN using the quantum recurrent block is as follows:
![[Pasted image 20260101164229.png]]
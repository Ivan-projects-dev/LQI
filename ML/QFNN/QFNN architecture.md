#ML #Quantum 
[[QFNN]] typically starts with a **feature map** $UF(→x)$ that transforms $|0⟩⊗n$ into an encoded state $|ψx⟩=UF(→x)|0⟩⊗n$. Common encoding methods include:
- **Rotation Encoding**: Use single-qubit rotations $(R_x, R_y, R_z)$ whose angles depend on the features.
- **[[Amplitude Encoding]]**: Map normalized data vectors into amplitudes of a multi-qubit state.
- **Basis Encoding**: Represent discrete features directly in qubit basis states.

After encoding, the network processes the state via one or $>$ **variational layers**. Each layer can consist of:
1. **Parametric Single-Qubit Rotations**: $Rμ(θ)$ gates on each qubit.
2. **Entangling Gates**: [[CNOT]], controlled-phase, or other multi-qubit operations to introduce Entanglement.
Typical [[QFNN]] may Stack multiple such layers, each with its own set of trainable params $θ$. The final layer may or may not be followed by additional gates depending on the design.

[[QFNN]]'s **output** is extracted by measuring the [[Qubits]]. In a supervised learning scenario:
- For **binary classification**, one might measure a single qubit in the $Z$-basis, interpreting measurement outcomes or expectation values $⟨Z⟩$ as the network's prediction.
- For **multi-class** or **regression** tasks, measuring multiple [[Qubits]] or computing $>$ complex observables yields higher-dimensional outputs.
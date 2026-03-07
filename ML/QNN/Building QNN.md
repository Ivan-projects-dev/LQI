#Math #Quantum #ML
**1. Data Preparation (Feature Map)**  
[[QNN]] begins with a **feature map**, which takes classical input data $→x$ and encodes it in a [[Quantum state]]. This is analogous to any preprocessing stage in classical neural networks but must respect the constraints of quantum computing. Common [[Feature maps]] include:
- **Angle/Rotation Encoding**: Applying single-qubit rotation gates (e.g., $R_y(2x_i)$) to embed each data component into a qubit's angles.
- **[[Amplitude Encoding]]**: Mapping data [[Vector]] elements to the amplitudes of a multi-qubit state, which requires normalization of the data.
- **Basis Encoding**: Using computational basis states to represent discrete features or categories.
**2. Variational Circuit (Processing)**  
After encoding, the network applies a **variational form** (or _ansatz_) with learnable params θ. This circuit is designed to "transform" the encoded state in a way reminiscent of how layers in a classical [[Neural network]] transform the activations. The [[QNN]]'s variational form may be structd in layers, each consisting of:
- **Parametric Single-Qubit Rotations**: For example, $R_y(θ)$ or $R_z(ϕ)$.
- **Entangling Gates**: Such as CNOT, $CZ$, or other $2$-qubit operations, arranged in a pattern (linear, cyclic, all-to-all) to introduce Entanglement between [[Qubits]].
In essence, [[Feature maps]] & variational forms are both **variational circuits**, but they differ in purpose. [[Feature maps]] depend on the input data, while the variational form depends on the optimizable params $θ$.
**3. Measurement & Output**  
Finally, measurements map the [[Quantum state]] back to a classical output. Depending on the [[QNN]]'s goal - e.g., binary classification, multi-class classification, or regression -one might measure:
- **Expectation value** of a certain operator (e.g., measuring Quantum states or the parity of multiple [[Qubits]]).
- **[[Probability distribution]]** over multiple measurement outcomes (e.g., for sampling or generative models).
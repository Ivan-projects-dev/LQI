#ML #Quantum 
**Quantum Feedforward Neural Networks (QFNNs)** replicate [[MLP]] architecture in a quantum setting by replacing the linear + nonlinear layers of a classical network with **quantum circuits**.
1. **Quantum Data Encoding**: Classical inputs $→x$ are embedded into an n-qubit [[Quantum state]] via a feature map $UF(→x)$.
2. **Variational Layers**: Instead of weight matrices & activation func, QFNNs use **parametrized quantum gates** (e.g., single-qubit rotations, multi-qubit entangling operations) as trainable layers; in other words, a variational form $U(θ)$.
3. **Quantum-to-Classical Readout**: Measurements of the final state provide the network's outputs for tasks like classification or regression.

Each quantum gate is a linear operator, so the **measurement** process plays the role of introducing effective nonlinearity. This makes QFNNs an intriguing blend of quantum circuit design & ML principles, mirroring the struct of classical feedforward nets but exec on quantum hardware.
![[Pasted image 20260101164447.png]]
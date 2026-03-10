#ML #Quantum 
An approach of a [[QCNN]] might look like this:
1. **Encoding**: Convert classical data (e.g., image) into init [[Quantum state]] via a feature map. Each pixel could be mapped to the amplitude or phase of a qubit, or a group of pixels might encode multiple [[Qubits]].
2. **Convolutional Layers**: Apply repeated _local_ gates across groups of neighboring [[Qubits]], with learnable params. Shift or alternate which [[Qubits]] are paired/clustered to capture different local patterns.
3. **[[QCNN]] Pooling layers**: Measure or entangle-and-discard subsets of [[Qubits]], reducing the circuit size to half (or another fraction). This acts as dimensionality reduction.
4. **Final Layer**: Once only a few [[Qubits]] remain, a "fully connected" block of gates (i.e., a $>$ global entangling operation) is applied. A final measurement yields the classification label, regression value, or other output.

Similar to [[QNN]] training elsewhere in this module, **gradient-based** or **gradient-free** approaches can be used. Each gate has learnable angles $θ$. One defines a cost func, such as mean-squared error or cross-entropy, & iteratively updates params to minimize it.

**Barren Plateaus**: Deep QCNNs may experience rapidly vanishing gradients. However, the layered "convolution + Pooling" struct can help keep circuit depth logarithmic in the system size, often mitigating this issue.
**Hardware Noise**: [[QCNN]] Pooling layers that involve measurement-based feedback can be challenging on current devices. One might prefer simulation or specialized hardware subroutines to test feasibility.
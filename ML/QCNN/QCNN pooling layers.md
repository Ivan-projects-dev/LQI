#ML #Quantum 
**Pooling layers** reduce the resolution of the maps by taking the max or average value within a small patch of pixels. This lowers the num of params, improves computational efficiency, & combats overfitting while retaining essential features.

Pooling layers, also known as subsampling, reduce dimensionality by reducing the num of params in the input. Like the convolutional layer, the Pooling operation applies a filter to the entire input, but the difference is that this filter has no weights. Instead, the kernel applies an Aggregation func to the values within the receptive field, populating the output [[Matrix]].

Directly discarding [[Qubits]] is nontrivial since quantum operations are unitary & linear. Instead, quantum Pooling can be realized by:
1. **Measuring Some [[Qubits]]**: Collapsing them to classical outcomes & using controlled gates on neighboring [[Qubits]]. The measurement acts as a nonlinear activation & reduces the num of active [[Qubits]].
2. **Entangle-Then-Discard**: A parametric gate entangles pairs of [[Qubits]], encoding essential info into Quantum states. The other qubit is subsequently "ignored" for subsequent layers.

**Measuring** or **discarding** [[Qubits]] effectively shrinks the circuit's size, analogous to classical Pooling. In practice, these steps can be repeated layer by layer to reduce the total active qubit count progressively.

One way to **reduce the qubit count** in a quantum circuit is to **pair up** the $N$ [[Qubits]]. Each pair undergoes a **generalized 2-qubit unitary**, effectively merging the info from both [[Qubits]] into the second qubit. The **first qubit** in each pair is then **ignored** for the rest of the network (no further gates or measurements apply to it).

This approach mimics Pooling by **"combining"** $2$ [[Qubits]]' info into one, thereby cutting the circuit size from $N$ to $N/2$. An alternative involves **dynamic circuits**, where measuring certain [[Qubits]] mid-circuit & feeding back measurement outcomes can also reduce the dimensionality. Either way, the goal is to **discard [[Qubits]]** that hold < essential info while preserving crucial features.

Below is an example showing how a $2$-qubit unitary can merge the state onto Quantum states, discarding the other:
![[Pasted image 20260101210022.png]]
By repeating this operation across pairs of [[Qubits]], the [[QCNN]] "pools" [[Qubits]] at each step, gradually **lowering the system size** in a manner analogous to classical Pooling layers.
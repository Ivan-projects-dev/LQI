#ML #Quantum 
Classical CNNs excel at tasks like image recognition by stacking **convolutional layers** (which detect local patterns) and **Pooling layers** (which reduce dimensions while preserving essential info). A final **fully connected layer** often aggregates high-level features to output a class label or regression result. These steps effectively learn filters that can detect edges, shapes, or other features in data such as images or audio.

The convolutional layer is the first layer in a convolutional network. While additional convolutional or Pooling layers can follow convolutional layers, the fully connected layer is the final layer. The CNN increases complexity with each layer, identifying $>$ significant %s of the image. The previous layers focus on simple features, such as colors & edges. As the image data progresses through the CNN layers, it begins to recognize $>$ prominent elements or shapes of the object until it finally identifies the desired object.
![Pasted image 20260101164351.png]
QCNNs strive to **replicate this approach quantum-mechanically**. Instead of sliding filters over pixel arrays, QCNNs apply carefully chosen **few-qubit gates** ("quantum filters") across neighboring [[Qubits]] & then use measurements or gate-based dimensionality reduction ("quantum Pooling") to compress the state. This allows the model to **extract local quantum correlations** and distill them into progressively fewer [[Qubits]], aiming to preserve key features while reducing the circuit size for subsequent layers.
![[Pasted image 20260101164406.png]]
One way to do it is as shown in the image above, where the input is an unknown [[Quantum state]] ρin, a convolutional layer is implemented by means of the repeated app of a few-qubit quantum gate $Ui$ in parallel. A Pooling layer reduces the size of the input measuring a fraction of the [[Qubits]] & acting with a gate $V_i$ on the nearby qubit controlled by the outcomes of the measurements. The measurement processes & the conditional action of $V_j$ implement the nonlinear activation func. The fully connected layer is represented by a gate $F$ on the remaining [[Qubits]]. The output is read measuring a fixed num of [[Qubits]]. The following quantum circuit is an example of a simple QCNN with $2$ convolutional layers & $2$ Pooling layers, the fully connected layer involves only $2$ [[Qubits]] with a final measurement process on a single qubit.

Num of convolutional & Pooling layers is fixed a priori & the gates $U_i, V_i, F$ are learned. Once defined a loss func, for example the mean-squared error in the output with respect to the training set, one can optimize the params of the variational circuit. To process a n-qubit input state, the num of params to learn is $O(logn)$.

The convolutional layer is the fundamental component of a CNN & is where most of the computation is performed. It requires input data, a filter, & a map. We also have a feature detector known as a kernel, which scans the image's receptive fields to check if the feature is present. This process is known as convolution.

A classical convolutional layer computes outputs x(l)ij from a region around pixels in the previous layer's map: $x(l)ij=W∑a,b=1wa,bx(l−1)i+a,j+b$ where $wa,b$ are learnable filter weights in a $W×W$ kernel.

![[Pasted image 20260101164309.png]]
In a **quantum convolutional layer**, we typically:
1. Identify small groups of neighboring [[Qubits]] (analogous to local patches/pixels).
2. Apply a **few-qubit unitary gate**-parametrized by angles θ-that "mixes" or "filters" these [[Qubits]].
3. Move to another local group (often overlapping) & apply the same or similar gates.

This approach captures local entanglement patterns. For example, a layer might consist of repeated 2-qubit or 3-qubit unitaries across the entire register, each gate effectively acting like a "quantum filter" on the local [[Qubits]].

An example of a pattern applies a $2$-qubit unitary to pairs $(q_0, q_1), (q_2, q_3)$, etc., followed by another pass on pairs $(q_1, q_2), (q_3, q_4)$, etc., reminiscent of "sliding" a filter over the qubit line or ring.

The proposal states that every unitary [[Matrix]] in $U(4)$ can be decomposed such that

$U=(A_1⊗A_2)⋅N(α,β,γ)⋅(A_3⊗A_4)$

where $A_j∈SU(2)$, $⊗$ is the [[Tensor]] product, and $N(α,β,γ)=exp(i[α_(σxσx)+β_(σyσy)+γ_(σzσz)])$, where $α,β,γ$ are the params that we can adjust.

Tuning this large amount of params would be difficult & would lead to long training times. To overcome this problem, we restrict our ansatz to a particular subspace of the Hilbert space & define the $2$ qubit unitary gate as $N(α,β,γ)$, which can serve as flexible local filter. These $2$-qubit unitaries, can be seen below & are applied to all neighboring [[Qubits]] each of the layers in the QCNN.
![[Pasted image 20260101164253.png]]

The image above shows a parametrized $2$-qubit unitary circuit for $N(α,β,γ)=exp(i[α_(σxσx)+β_(σyσy)+γ_(σzσz)])$, where $α=π^2−2_θ$, $β=2ϕ−π^2$ and $γ=π^2−2λ$.
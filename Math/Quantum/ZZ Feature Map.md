#Quantum #ML 
It is commonly used in [[QSVM]] contexts, yet it can serve generally as a feature map for QNNs. The core idea is to encode classical data $→x$ by applying rotations on individual [[Qubits]], then **introducing entangling phases** using interactions of the form:

$exp(−i_γx_jx_kZ_jZ_k)$ where $Z_i$ and $Z_j$ are Pauli-Z operators on [[Qubits]] $i$ and $j$, respectively, and $γ$ is some scaling param. Repeated app of such entangling terms, interspersed with single-qubit rotations, yields a feature map that can capture higher-order correlations between features. This "ZZ Feature Map" effectively places the data in a high-dimensional Hilbert space in a nonlinear fashion-potentially allowing the [[QNN]] to learn complex decision boundaries.

This feature map can take $n$ inputs $x_1, …, x_n$ on $n$ [[Qubits]]. Its parametrized circuit is constructed following these steps:
1. Apply a Hadamard gate on each qubit.
2. Apply, on each qubit j, a phase gate P(2xj).
3. For each pair of elements ${j,k}⊆{1,…,n}$ with $j<k$, do the following:
	- Apply a CNOT gate targeting qubit $k$ and controlled by qubit $j$.
	- Apply, on qubit $k$, a phase gate $P(2(π−xj)(π−xk))$.
	- Repeat step 3.1
As with angle encoding, normalization plays a big role in the ZZ feature map. It guarantees a healthy balance between separating the extrema of the dataset & using as big a region as possible in the feature space.
![[Pasted image 20260101214717.png]]

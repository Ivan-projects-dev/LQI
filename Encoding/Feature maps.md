#Quantum #ML #Math 
**Feature maps** are procedures that **embed classical data** into quantum states, providing a starting point for the variational circuit that follows. 

Angle/rotation encoding maps each component $x_i$ (feature) of the input [[Vector]] $→x$ to the rotation angle of a single-qubit gate. A simple example is using a $R_y$ gate & the value of each feature directly:  $|0⟩R_y(2x_i)→cos(x_i)|0⟩+sin(x_i)|1⟩$

If multiple features need to be encoded, one can apply separate [[Rotation gates]] on different [[Qubits]] or on the same qubit(s) sequentially.

**Angle encoding** is conceptually straightforward & hardware-friendly, as it relies primarily on [[Single-qubit gates]].

In a general way, the angle encoding scheme can be summarized as follows. We assume we have a $k$-feature dataset consisting of $M$ samples & all features $x_1,…,x_k$ are real-valued:
$→x_j:=(x_(j1),…,x_(jk))∈R_k,j=1,…,M$

Angle encoding works by constructing the map: $→x_j↦k⨂i=1(cos(φ_(ji2))|0⟩+sin(φ_(ji2))|1⟩)$
where angles $(φ_(ji))i=1,…,k;j=1,…,N$ are given by the expression
$φ_(ji)=x_(ji)−xmin_ixmax_i−xmin_iπ4$

where $xmin_i=min_jx_(ji)$ and $xmax_i=max_jx_(ji)$. This normalization considers the $φ∈[0,π]$ angle restriction (due to the [[Bloch sphere]]) & ensures that each feature value has a unique rotation.

This scheme only requires one rotation gate for each qubit, hence encodes as many features as the num of [[Qubits]] ($n$ features using $n$ [[Qubits]]).

We can use any rotation gate of our choice; however, if we use $R_z$ gates & take $|0⟩$ to be our init state, the action of our feature map will have no effects. That is why, when $R_z$ gates are used, it is customary to precede them by Hadamard gates.
![[Pasted image 20260101205342.png]]
On the other hand, we know that a single quantum register can encode $2$ real vars. Recall the [[Bloch sphere]], where we need the angles $θ∈[0,π]$ and $ϕ∈[0,2π)$ to represent a state of a qubit: $|ψ⟩=cos(θ_2)|0⟩+eiϕsin(θ_2)|1⟩$.

The following scheme maps the classical sample into the [[Quantum state]] with the help of an extra phase gate $(R_z)$:
$→xj↦k⨂i=1(cos(φj2i−12)|0⟩+exp(iφ2i)sin(φj2i−12)|1⟩)$
This scheme allows us to encode $2_n$ features using $n$ [[Qubits]].
![[Pasted image 20260101205413.png]]
![[Pasted image 20260101214626.png]]
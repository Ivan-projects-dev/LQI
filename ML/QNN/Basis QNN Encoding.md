#ML #Quantum #Math 
In basis encoding, each feature (often a discrete or binary var) is mapped directly to qubit basis states. For instance, a binary input [[Vector]] $b=(b_1,…,b_k)$ with $b_i∈{0,1}$ can be represented as $|b_1b_2…b_k⟩$ (in the computational basis)
Each data point has to be a $N$-bit binary string: $→x_j=(b_1,…,b_N)$

Assuming all features are represented with unit binary precision (one bit), each input example $→x_j$ can be directly mapped to the [[Quantum state]] $|→x_j⟩$.

An entire dataset, with $M$ samples, can be represented in superpositions of computational basis states as $|D⟩=1√MM∑j=1|→x_j⟩|D⟩=1√MM∑j=1|→x_j⟩$

For $N$ bits, there are $2N$ possible basis states. Given $M<2N$, the basis embedding of $D$ will be sparse.

This encoding is especially natural for binary inputs or categorical data with limited cardinalities. However, continuous or high-dimensional inputs typically require additional transformations or approximations.
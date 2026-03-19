#Quantum #Algorithm #Physics #Math 
**Amplitude encoding** represents data points as the amplitudes of a multi-qubit [[Quantum state]]. Given an N-dimensional (with $N=2n$) data point: $→x=(x_1, x_2, … ,x_N)∈CN$ we map it to $|ψ(→x)⟩=1√∑kx2kN−1∑j=0xj|j⟩$ where $|j⟩$ denotes the $j$th computational basis state of $n$ [[Qubits]]. 

We can therefore encode the dataset $D:=(→x_1,…,→x_M)$ consisting of $M$ points in $R2n$ as
$|D⟩=1CD2p∑i=1¯X_i|i⟩$ for some int $p$, where
$$¯X=(¯Xi)i=1,…,2p=(x11,…,x1N,x21,…,x2N,…,xM1,…,xMN)∈RMN$$
is the concatenation of all the data points and $CD$ is a normalization const. Here the constraint is hence that $2p≥MN$, namely $p≥log2(MN)$

Note that there may be some sparsity in the case where $2p>MN$.
The clear advantage is that it can store $2n$ features with only n [[Qubits]], but unfortunately has a depth $O(2n)$ and is hence hard to build.

Amplitude encoding is memory-efficient in $T.$, since it can pack exponentially many classical components into n [[Qubits]]. However, preparing an arbitrary amplitude-encoded state can be resource-intensive & is usually feasible only for lower-dimensional data (or carefully structd data).
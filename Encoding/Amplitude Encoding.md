#Algorithm #Physics #Math 
**Amplitude encoding** represents data points as the amplitudes of a multi-qubit [[Quantum state]]. Given an $N$-dimensional data point ($N = 2^n$):
$$\vec{x} = (x_1, x_2, \ldots, x_N) \in \mathbb{C}^N$$
we map it to
$$|\psi(\vec{x})\rangle = \frac{1}{\|\vec{x}\|} \sum_{j=0}^{N-1} x_j |j\rangle$$
where $|j\rangle$ denotes the $j$th computational basis state of $n$ [[Qubits]] and $\|\vec{x}\| = \sqrt{\sum_k |x_k|^2}$ is the normalization.

We can encode a dataset $D := (\vec{x}_1, \ldots, \vec{x}_M)$ of $M$ points in $\mathbb{R}^{2^n}$ as
$$|D\rangle = \frac{1}{C_D} \sum_{i=1}^{2^p} \bar{X}_i |i\rangle$$
for some integer $p$, where
$$\bar{X} = (x_{11}, \ldots, x_{1N},\; x_{21}, \ldots, x_{2N},\; \ldots,\; x_{M1}, \ldots, x_{MN}) \in \mathbb{R}^{MN}$$
is the concatenation of all data points and $C_D$ is the normalization constant. The constraint is $2^p \geq MN$, i.e. $p \geq \log_2(MN)$.

Note that there may be some sparsity in the case where $2^p > MN$.

**Advantage:** stores $2^n$ features using only $n$ [[Qubits]] - exponential compression in qubit count.

**Disadvantage:** preparing an arbitrary amplitude-encoded state requires circuit depth $O(2^n)$, making it expensive to build for general data. State preparation is usually feasible only for low-dimensional or structured data, or with QRAM hardware support.

Amplitude encoding is memory-efficient in qubit count, but the state preparation cost can dominate and must be included when evaluating any claimed quantum speedup.

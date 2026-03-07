#Quantum #Math 
Overall probability must be $1$ when we observe quantum system.

For example, the following vectors cannot be a valid quantum state:
$$\begin{pmatrix}
1/2 \\
1/2
\end{pmatrix}, \begin{pmatrix}
√3/2 \\
1/√2
\end{pmatrix}$$
For the first [[Vector]], the probabilities of observing the states $|0⟩$ and $|1⟩$ are $14$.

So, the overall probability of getting a result is $14+14=12$, which is $< 1$.
For the second Vector, the probabilities of observing the states $|0⟩$ and $|1⟩$ are respectively $34$ and $12$.
So, the overall probability of getting a result is $34+12=54$, which is $> 1$.

**The summation of amplitude squares must be $1$ for a valid quantum state.**
**$>$ formally, a quantum state can be represented by a [[Vector]] having length $1$, & vice versa.**
The summation of amplitude squares gives the square of the length of [[Vector]].
But, this summation is $1$, & its square root is also $1$. So, we can use the term length in the definition.
**Technical notes:** We represent a quantum state as $|u⟩$ instead of $u$. Remember the relation between the length & dot product: $∥u∥=√u⋅u$.

In quantum computation, we use inner product instead of dot product, which is defined on [[Complex nums]]. By using bra-ket notation, $∥|u⟩∥=√⟨u|u⟩=1$, or equivalently $⟨u|u⟩=1$, where $⟨u|u⟩$ is a short form of $⟨u||u⟩$. For real-valued vectors, $⟨v|v⟩=v⋅v$.

**Any length preserving (square) [[Matrix]] is a quantum operator, & vice versa.**
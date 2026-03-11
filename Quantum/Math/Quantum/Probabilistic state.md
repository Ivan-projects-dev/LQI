#Quantum #Math 
Suppose that we have a system with $4$ distinguishable states: $s_1$, $s_2$, $s_3$, and $s_4$. We expect the system to be in one of them at any moment.

System is in one of the states with probability $1$, & in any other state with probability $0$.
By using our column representation, we can show each state as a column [[Vector]] (by using the vectors in standard basis of $R^4$):
$$e_1 = \begin{pmatrix}
1 \\
0 \\
0 \\
0
\end{pmatrix}, e_2 = \begin{pmatrix}
0 \\
1 \\
0 \\
0
\end{pmatrix}, e_3 = \begin{pmatrix}
0 \\
0 \\
1 \\
0
\end{pmatrix}, e_4 = \begin{pmatrix}
0 \\
0 \\
0 \\
1
\end{pmatrix}$$
This representation helps us to state our info on a system when it is in $>$ than one state with certain probabilities. Remember the case in which the coins are tossed secretly.

**Probabilistic state** is a linear combo of the vectors in the standard basis.

Coefficients (**scalars**) must satisfy certain properties:
1. Each coefficient is non-negative
2. The summation of coefficients is $1$
Alternatively, we can say that a probabilistic state is a [[Probability distribution]] over deterministic states. We can show all info as a single math object - **stochastic [[Vector]]**.

The state of any linear system is linear combo of the vectors in the basis.
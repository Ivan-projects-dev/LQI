#Quantum #Math 
[[Matrix]] form of the controlled-NOT operator is as follows:
$$CNOT = \begin{pmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0
\end{pmatrix} = \begin{pmatrix}
I & 0 \\
0 & X
\end{pmatrix}$$
where $X$ denotes $NOT$ operator.
Similarly, for a given single bit operator $M$, we can define the **controlled-$M$ operator** (where the first bit is the control bit & the second bit is target bit) as follows:
$$CM = \begin{pmatrix}
I & 0 \\
0 & M
\end{pmatrix}$$
By definition:
- when the first bit is $0$, the identity is applied to the $2nd$ bit, and
- when the first bit is $1$, the operator $M$ is applied to the $2nd$ bit.
Here we observe that the [[Matrix]] CM has a nice form because the first bit is control bit. The [[Matrix]] CM given above is divided into $4$ sub-matrices based on the states of the first bit. Then, we can follow that
- the value of the first bit never changes, & so the off diagonal sub-matrices are $0$s;
- when the first bit is $0$, the identity is applied to the second bit, & so top-left [[Matrix]] is $I$; and,
- when the first bit is $1$, the operator $M$ is applied to the second bit, & so the bottom-right [[Matrix]] is $M$.
For a given single bit operator $M$, **how can we obtain the following operator** by using the operator $CM$?
$$C_0M=\begin{pmatrix}
M & 0 \\
I & 0
\end{pmatrix}$$
Controlled operator are defined to be triggered when the control bit is in state $1$. In this example, we expect it to be triggered when the control bit is in state $0$.

Here we can use a simple trick. We first apply $NOT$ operator to the first bit, & then the $CM$ operator, & again $NOT$ operator. In this way, we guarantee that $M$ is applied to the second bit if the first bit is state $0$ & do nothing if the first bit is in state $1$. In short:
$C_0M=(X⊗I)⋅(CM)⋅(X⊗I)$
#Math 
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
Similarly, for given single bit operator $M$, we can define the **controlled-$M$ operator** (where the $1st$ bit is the control bit & the $2nd$ bit is target bit) as follows:
$$CM = \begin{pmatrix}
I & 0 \\
0 & M
\end{pmatrix}$$
By definition:
- when the $1st$ bit is $0$, the identity is applied to the $2nd$ bit, &
- when the $1st$ bit is $1$, the operator $M$ is applied to the $2nd$ bit.

Here we observe that the [[Matrix]] CM has nice form because the $1st$ bit is control bit. 

[[Matrix]] CM given above is divided into $4$ sub-matrices based on the states of the $1st$ bit. Then, we can follow that
- the value of the $1st$ bit never changes, & so the off diagonal sub-matrices are $0$s;
- when the $1st$ bit is $0$, the identity is applied to the $2nd$ bit, & so top-left [[Matrix]] is $I$; &,
- when the $1st$ bit is $1$, the operator $M$ is applied to the $2nd$ bit, & so the bottom-right [[Matrix]] is $M$.

For given single bit operator $M$, **how can we obtain the following operator** by using the operator $CM$? $$C_0M=\begin{pmatrix}M & 0 \\0 & I\end{pmatrix}$$Controlled operator defined to be triggered when the control bit is in state $1$. In this example, we expect it to be triggered when the control bit is in state $0$.

We apply $NOT$ operator to the $1st$ bit, & then the $CM$ operator, & again $NOT$ operator. In this way, we guarantee that $M$ is applied to the $2nd$ bit if the $1st$ bit is state $0$ & do nothing if the $1st$ bit is in state $1$. In short: $$C_0M=(X⊗I)⋅(CM)⋅(X⊗I)$$
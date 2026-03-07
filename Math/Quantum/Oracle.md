#Math #Quantum 
Suppose that there exists a func $f:{0,1}n→{0,1}$ with the following properties: $f(x)=1$ if $x$ is marked $f(x)=0$ otherwise

Grover algorithm does not actually search a list of elements, but given func $f$ with the above properties, it finds the element $x$ such that $f(x)=1$.

$f$ is often called as the **oracle/blackbox**. Even though $f$ might not be reversible, it can be implemented in a reversible manner by using the following idea.

Here $U_f$, the corresponding quantum operator is defined as follows, where $⊕$ denotes bitwise addition modulo $2$ (XOR).
$U_f:|x⟩|y⟩↦|x⟩|y⊕f(x)⟩$
Note that this mapping is reversible. When $|y⟩=|0⟩$, you get exactly $f(x)$ in the output qubit.

Assume that $|y⟩=|−⟩=1√2(|0⟩−|1⟩)$ and investigate the effect of the operator $U_f$.
$$U_f|x⟩|−⟩=U_f|x⟩1√2(|0⟩−|1⟩)=1√2(U_f|x⟩|0⟩−U_f|x⟩|1⟩)=1√2(|x⟩|f(x)⊕0⟩−|x⟩|f(x)⊕1⟩)=|x⟩1√2(|f(x)⟩−|f(x)⊕1⟩)=|x⟩(−1)f(x)1√2(|0⟩−|1⟩)=(−1)f(x)|x⟩|−⟩$$
We have the following transformation: $|x⟩|−⟩Uf−→(−1)f(x)|x⟩|−⟩$

When $f(x)=1$, we see that a phase of $-1$ is kicked back to the front of the first register. Hence by preparing the output register in state $|−⟩$ and applying $U_f$, we obtain the **sign flip** effect.

Note that even if we don't know anything about $f$ (that's why it is called blackbox), we are able to flip the sign of the amplitude of the marked element by making query to $f$ by setting output qubit to $|−⟩$

Oracle func $f$ depends on the problem, it is possible to model many different problems (such as graph coloring, traveling salesman & many $>$) as search problem. Elements in search space correspond to quantum states. Instead of searching the whole space, you design $f$ so that it checks whether an element in the search space is the actual solution & marks it by outputting $1$.
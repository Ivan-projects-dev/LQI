#Algorithm #Quantum #Q-Sharp
[[Simon's algorithm]] oracles implement $|x\rangle|y\rangle \to |x\rangle|y\oplus f(x)\rangle$. For the algorithm to work, $f$ must satisfy the two-to-one property with a hidden period $s$.

**[[Oracle]] types**:

_CountBits_ ($f(x) = x_0\oplus x_1\oplus\cdots\oplus x_{n-1}$, i.e. parity) - output register is a single qubit $y$. Apply [[CNOT]] from each $x_i$ to $y$. Hidden period: any $s$ with even weight (multiple valid $s$, but problem asks for one specific one). This is a one-to-many case; used as [[Oracle]] warm-up.

_BitwiseRightShift_ ($f(x)_0 = 0$, $f(x)_i = x_{i-1}$ for $i>0$) - output register is $n$ [[Qubits]]. Copy each bit one position right: [[CNOT]] from $x[i]$ to $y[i+1]$ for $i=0,\ldots,n-2$. Hidden period $s = 0\underbrace{0\cdots0}_{n-2}1$ (the last bit is $1$, all others $0$), since $f(x) = f(x\oplus s)$.

_LinearOperator_ ($f(x) = A\cdot x \pmod 2$, single output qubit) - $A$ is a $1\times n$ binary row [[Vector]]. Apply [[CNOT]] from $x[c]$ to $y$ for each column $c$ where $A[c]=1$. For Simon's, $A$ defines a hyperplane; $s$ is any [[Vector]] in the kernel of $A$ over $\mathbb{F}_2$.

_MultidimensionalLinearOperator_ ($f(x) = A\cdot x \pmod 2$, $n_2$-qubit output) - $A$ is $n_2\times n_1$. Apply [[CNOT]] from $x[c]$ to $y[r]$ for each $(r,c)$ where $A[r][c]=1$. The hidden period $s$ lies in the null space of $A$ over $\mathbb{F}_2$.

**General [[Oracle]] structure**: always of the form "XOR contribution of $x$ into $y$" → each output bit $r$ gets XORed with the inner product $A_r\cdot x$, implemented as a fan-out of CNOTs. No [[Ancilla]] is needed & the [[Oracle]] is automatically reversible (its own adjoint).

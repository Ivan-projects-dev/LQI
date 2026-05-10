#Math 
**iSWAP** applies phase of $i$ during the [[SWAP]]:
$$iSWAP = \begin{pmatrix}1&0&0&0\\0&0&i&0\\0&i&0&0\\0&0&0&1\end{pmatrix}$$
$iSWAP$ is the native $2$-qubit gate on some superconducting & photonic platforms. It can simulate [[CNOT]] with additional [[Single-qubit gates]].

$sqrt$[[SWAP]]: $1/2$-[[SWAP]] - entangles [[Qubits]] without fully exchanging them. Applying $sqrt$[[SWAP]] twice gives [[SWAP]]. Together with [[Single-qubit gates]], $sqrt$[[SWAP]] is universal:
$$\begin{pmatrix}1&0&0&0\\0&\frac{1+i}{2}&\frac{1-i}{2}&0\\0&\frac{1-i}{2}&\frac{1+i}{2}&0\\0&0&0&1\end{pmatrix}$$
**Controlled [[SWAP]]** swaps $2$ [[Qubits]] conditioned on $3rd$:
$$CSWAP: |c\,ab\rangle \mapsto \begin{cases}|c\,ab\rangle & c = 0 \\ |c\,ba\rangle & c = 1\end{cases}$$
$8\times8$ unitary with $CSWAP^2 = I$. [[T gate]] cost: $7$ (same as [[Toffoli]]). 
Decomposition: $CSWAP = CNOT_{ba} \cdot CCNOT_{c,a,b} \cdot CNOT_{ba}$.

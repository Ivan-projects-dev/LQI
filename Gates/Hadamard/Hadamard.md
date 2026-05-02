#Math #Q-Sharp #Python 
**Hadamard gate** $H$ is the single most important gate in quantum computing. It creates superposition from basis state & is the bridge between **computational basis** $\{|0\rangle, |1\rangle\}$ & **Hadamard basis** $\{|+\rangle, |{-}\rangle\}$.

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix}1 & 1\\1 & -1\end{pmatrix}$$
$$H|0\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}} = |{+}\rangle \qquad H|1\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}} = |{-}\rangle$$
Applied to $|0\rangle$ it creates **uniform superposition**: both outcomes $0$ & $1$ are equally likely on measurement. Applied to $|1\rangle$ it creates the same uniform superposition but with a **relative phase flip** between the two terms - the phase encodes classical info that a quantum algorithm can exploit.

$H^2 = I$, so applying $H$ $x2$ returns the qubit to its original state. This also means the **adjoint is itself**: `Adjoint H = H` in Q#
$$H \cdot H = \frac{1}{2}\begin{pmatrix}1 & 1\\1 & -1\end{pmatrix}\begin{pmatrix}1 & 1\\1 & -1\end{pmatrix} = \begin{pmatrix}1 & 0\\0 & 1\end{pmatrix}$$
This makes $H$ both unitary & Hermitian - rare combination.

$H$ defines natural alternative orthonormal basis for qubit:
$$|{+}\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}} \qquad |{-}\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}$$

| State          | Computational basis representation            | Measurement outcome                    |
| -------------- | --------------------------------------------- | -------------------------------------- |
| $\|{+}\rangle$ | $\frac{1}{\sqrt{2}}(\|0\rangle + \|1\rangle)$ | $50$% $\|0\rangle$, $50$% $\|1\rangle$ |
| $\|{-}\rangle$ | $\frac{1}{\sqrt{2}}(\|0\rangle - \|1\rangle)$ | $50$% $\|0\rangle$, $50$% $\|1\rangle$ |
Both states look **identical** when measured in the computational basis - all info about which state it is sits in the phase. $H$ is the operation that makes that phase readable.

On the Bloch sphere, $H$ is a $180°$ rotation about the diagonal $XZ$ axis $(\hat{x} + \hat{z})/\sqrt{2}$:
- North pole $|0\rangle$ $\to$ equator $+X$ pole $|{+}\rangle$
- South pole $|1\rangle$ $\to$ equator $-X$ pole $|{-}\rangle$
- $+Z$ & $+X$ poles swap; $-Z$ & $-X$ poles swap
Applying $H$ then $Z$ then $H$ equals $X$: this is the **basis conjugation identity** $HZH = X$, $HXH = Z$, which is used constantly to convert between Pauli errors.
### n-qubit Hadamard: $H^{\otimes n}$
Applying $H$ to each qubit of an $n$-qubit $|0\rangle^{\otimes n}$ register simultaneously creates a **uniform superposition over all $2^n$ computational basis states**:
$$H^{\otimes n}|0\rangle^{\otimes n} = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}|x\rangle$$
This single operation in $O(n)$ gates encodes all possible inputs - what would take $2^n$ steps classically. It is the starting point of Grover, Deutsch-Jozsa, Bernstein-Vazirani, Simon, & virtually every other quantum algorithm.
More generally, for any $n$-bit string $s$:
$$H^{\otimes n}|s\rangle = \frac{1}{\sqrt{2^n}}\sum_{x=0}^{2^n-1}(-1)^{s \cdot x}|x\rangle$$
where $s \cdot x = s_0 x_0 \oplus \cdots \oplus s_{n-1}x_{n-1}$ is the bitwise inner product. This is the **Walsh-Hadamard transform** - real-valued QFT over $\mathbb{Z}_2^n$.

| Algorithm          | How $H$ is used                                                                                                                    |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| Grover         | $H^{\otimes n}$ creates uniform superposition; $H^{\otimes n} (2\|0\rangle\langle0\| - I) H^{\otimes n}$ is the diffusion operator |
| QPE            | $H^{\otimes t}$ creates clock register superposition; inverse QFT ≈ repeated $H$ layers                                        |
| Simon          | $H^{\otimes n}$ before & after oracle; interference pattern reveals hidden period                                                  |
| Bernstein-Vazirani | $H^{\otimes n}$ both sides of oracle; single query recovers $n$-bit secret string                                                  |
| Teleportation      | $H$ on Bell measurement converts phase into bit, enabling classical correction                                                     |
| Deutsch-Jozsa      | $H^{\otimes n}$ creates superposition; output $\|0\rangle^n$ iff $f$ is constant                                                   |
$$HXH = Z \qquad HZH = X \qquad HYH = -Y$$
This means: **Hadamard conjugation swaps $X$ & $Z$ errors.** $Z$ error on qubit before $H$ is equivalent to an $X$ error after - important for error correction analysis.
$$H = \frac{X + Z}{\sqrt{2}}$$
$H$ can also be decomposed in terms of rotations: $H = R_y(\pi/2) \cdot Z = R_y(-\pi/2) \cdot X$.
```csharp
H(q); // |0⟩ → |+⟩, |1⟩ → |−⟩
Adjoint H(q); // identical to H since H² = I
ApplyToEach(H, register); // H⊗n on qubit array
```
Preparing $|{-}\rangle$ for phase kickback:
```csharp
use anc = Qubit();
X(anc);
H(anc); // anc is now |−⟩
// ... run oracle with anc as target ...
H(anc);
X(anc);
Reset(anc);
```
**Qiskit**
```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(3)
qc.h(0) # H on qubit 0
qc.h([0, 1, 2]) # H on all three qubits
```
**PennyLane**
```python
import pennylane as qml

@qml.qnode(dev)
def circuit():
    qml.Hadamard(wires=0)
    qml.Hadamard(wires=[0, 1, 2]) # applied to each
    return qml.state()
```

| Property | Value |
|---|---|
| [[Matrix]] | $\frac{1}{\sqrt{2}}\begin{pmatrix}1 & 1\\\\1 & -1\end{pmatrix}$ |
| Unitary | Yes |
| Hermitian | Yes |
| Self-inverse | Yes ($H^2 = I$) |
| Bloch rotation | $180°$ about $(\hat{x}+\hat{z})/\sqrt{2}$ |
| Q# name | `H` |
| Qiskit name | `h` |
| [[PennyLane]] name | `qml.Hadamard` |
| [[T gate]] count | $0$ (Clifford gate) |

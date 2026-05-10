#Math #Q-Sharp #Python 
**Pauli-X gate** is the quantum analogue of the classical NOT gate. It flips $|0\rangle \leftrightarrow |1\rangle$ & is the most basic single-qubit operation.
$$X = \begin{pmatrix}0 & 1\\1 & 0\end{pmatrix}$$
$$X|0\rangle = |1\rangle \qquad X|1\rangle = |0\rangle$$
On superpositions it swaps amplitudes: $X(\alpha|0\rangle + \beta|1\rangle) = \beta|0\rangle + \alpha|1\rangle$.

$X^2 = I$ - applying $X$ twice returns the qubit to its original state, so $X$ is both **self-inverse & Hermitian**. The adjoint is itself: `Adjoint X = X`.

$X$ is a **Pauli [[Matrix]]**: $X^2 = Y^2 = Z^2 = I$ & $XY = iZ$, $YZ = iX$, $ZX = iY$.
$$X = HZH \qquad X = -iYZ \qquad X = R_x(\pi) \cdot e^{i\pi/2}$$
[[Hadamard]] conjugation $HXH = Z$ means: $X$ error before $H$ becomes $Z$ error after. This identity is used constantly in error correction analysis to track Pauli errors through circuits.

$X$ is in the **Clifford group** - it maps Pauli operators to Pauli operators under conjugation & can be corrected by stabilizer error-correcting codes. [[T gate]] cost is $0$.
### Bloch sphere geometry
$X$ is $180°$ rotation about the $\hat{x}$ axis. North pole $|0\rangle$ maps to south pole $|1\rangle$ & vice versa. The equatorial states $|{+}\rangle$ & $|{-}\rangle$ are fixed points of this rotation:
$$X|{+}\rangle = |{+}\rangle \qquad X|{-}\rangle = -|{-}\rangle$$
(The $|{-}\rangle$ eigenvalue $-1$ means $X$ picks up a phase on that eigenstate - this is the [[Phase kickback]] mechanism exploited in [[Hadamard#Phase kickback|oracles]].)

| Eigenvalue | Eigenstate                                                   |
| ---------- | ------------------------------------------------------------ |
| $+1$       | $\|{+}\rangle = \frac{1}{\sqrt{2}}(\|0\rangle + \|1\rangle)$ |
| $-1$       | $\|{-}\rangle = \frac{1}{\sqrt{2}}(\|0\rangle - \|1\rangle)$ |
Measuring in the $X$ basis means applying [[Hadamard]] then measuring in the computational basis.

`Controlled X([ctrl], target)` is exactly [[CNOT]]:
$$CX = \begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&0&1\\0&0&1&0\end{pmatrix}$$
This is the most important $2$-qubit gate. See [[CNOT]] for the full treatment.

$n$-controlled $X$ is the Toffoli-like gate: `Controlled X([c0, c1, ..., cn-1], target)`.

$X$ appears in virtually every quantum circuit in $2$ roles:
1. **Initialization**: preparing $|1\rangle$ from $|0\rangle$ before ancilla use, or preparing specific computational basis states.
2. **Oracle construction**: inside oracles, $X$ gates are placed before & after multi-controlled operations to mark specific bit patterns - the standard `within { X(q[i]); } apply { CCNOT(...) }` pattern.

```csharp
X(q); // |0⟩ → |1⟩, |1⟩ → |0⟩
Controlled X([ctrl], target); // CNOT
Controlled X([c0, c1], t); // Toffoli
ApplyToEach(X, register); // flip all qubits
```
Bit-pattern marking (oracle):
```csharp
within {
    // flip qubits that should be 0 in the target pattern
    for i in negativeVars { X(register[i]); }
}
apply {
    Controlled X(register, ancilla); // fires only on target pattern
}
```
**Qiskit**
```python
qc.x(0) # X on qubit 0
qc.cx(0, 1) # CNOT: control=0, target=1
qc.ccx(0, 1, 2) # Toffoli
```
**PennyLane**
```python
qml.PauliX(wires=0)
qml.CNOT(wires=[0, 1])
```

| Property | Value |
|---|---|
| [[Matrix]] | $\begin{pmatrix}0 & 1\\\\1 & 0\end{pmatrix}$ |
| Unitary | Yes |
| Hermitian | Yes |
| Self-inverse | Yes ($X^2 = I$) |
| Clifford gate | Yes |
| Bloch rotation | $180°$ about $\hat{x}$ |
| [[T gate]] count | $0$ |
| Q# name | `X` |
| Qiskit name | `x` |
| [[PennyLane]] name | `qml.PauliX` |

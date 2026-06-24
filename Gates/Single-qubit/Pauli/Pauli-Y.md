#Gate #Math
**Pauli-Y gate** is one of the three [[Pauli group|Pauli gates]], represented by the matrix:
$$Y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}$$

### Action on basis states
$$Y|0\rangle = i|1\rangle \qquad Y|1\rangle = -i|0\rangle$$

Y combines a bit-flip (like X) with a phase-flip (like Z): it flips the qubit and multiplies by $\pm i$.

### Bloch sphere
Y rotates $180°$ around the $\hat{y}$-axis. Combined with [[Pauli-X]] ($\hat{x}$) and [[Pauli-Z]] ($\hat{z}$), the three Paulis generate all single-qubit rotations.

### Eigenstates
$$|{+i}\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix}1\\i\end{pmatrix}, \quad Y|{+i}\rangle = +|{+i}\rangle$$
$$|{-i}\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix}1\\-i\end{pmatrix}, \quad Y|{-i}\rangle = -|{-i}\rangle$$

These are the $\pm\hat{y}$ poles of the Bloch sphere.

### Properties
| Property | Value |
|---|---|
| Hermitian | Yes ($Y^\dagger = Y$) |
| Unitary | Yes ($Y^\dagger Y = I$) |
| Involutory | Yes ($Y^2 = I$) |
| Eigenvalues | $+1$, $-1$ |
| Trace | $0$ |
| Determinant | $+1$ |

**Commutation**: $[X, Y] = 2iZ$, $[Y, Z] = 2iX$, $[Z, X] = 2iY$ (cyclic).

**Anticommutation**: $\{Y, X\} = \{Y, Z\} = 0$ — Paulis anticommute pairwise.

### Where Y appears
- **Pauli error channels**: $Y$ error = simultaneous bit-flip + phase-flip; appears in depolarizing noise model alongside $X$ and $Z$ errors
- **Rotation gate**: $R_y(\theta) = e^{-i\theta Y/2} = \cos(\theta/2)I - i\sin(\theta/2)Y$ — Y is the generator of $y$-axis rotations
- **Clifford group**: Y is a Clifford gate; $YXY^\dagger = -X$, $YZY^\dagger = -Z$
- **Imaginary unit**: Y is the only Pauli with purely imaginary entries; relevant in time-reversal symmetry

```qsharp
// Q#
operation ApplyY(q : Qubit) : Unit is Adj + Ctl {
    Y(q);
}
```

```python
# Qiskit
from qiskit import QuantumCircuit
qc = QuantumCircuit(1)
qc.y(0)

# PennyLane
import pennylane as qml
qml.PauliY(wires=0)
```

Source: [Microsoft.Quantum.Intrinsic.Y — MS Learn](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.intrinsic/y) [YGate — Qiskit](https://docs.quantum.ibm.com/api/qiskit/qiskit.circuit.library.YGate)

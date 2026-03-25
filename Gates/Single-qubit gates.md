#Q-Sharp #Quantum #Math
**Single-qubit gates** are unitary $2\times2$ matrices acting on $1$ [[Qubits|qubit]]. All are available in `Std.Intrinsic` & support `Adj + Ctl`.

**Pauli-X (NOT) - `X`**
$$X = \begin{pmatrix}0 & 1\\1 & 0\end{pmatrix}$$
Flips $|0\rangle \leftrightarrow |1\rangle$. Classical NOT. $X^2 = I$.

```csharp
X(q); // |0⟩ → |1⟩,  |1⟩ → |0⟩
Controlled X([c], q); // CNOT: flips q iff c == |1⟩
```

**Pauli-Y - `Y`**
$$Y = \begin{pmatrix}0 & -i\\i & 0\end{pmatrix}$$

$|0\rangle \mapsto i|1\rangle$, $|1\rangle \mapsto -i|0\rangle$. $Y = iXZ$. $Y^2 = I$.

```csharp
Y(q);
```

**Pauli-Z (phase flip) - `Z`**
$$Z = \begin{pmatrix}1 & 0\\0 & -1\end{pmatrix}$$
Leaves $|0\rangle$ unchanged; flips sign of $|1\rangle$. $Z|{+}\rangle = |{-}\rangle$. $Z^2 = I$.

```csharp
Z(q);
Controlled Z([c], q); // CZ gate: symmetric - either qubit can be control
```

**Hadamard - `H`**
$$H = \frac{1}{\sqrt{2}}\begin{pmatrix}1 & 1\\1 & -1\end{pmatrix}$$
$|0\rangle \mapsto |{+}\rangle = \frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)$, $|1\rangle \mapsto |{-}\rangle = \frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)$. Creates & destroys superposition. $H^2 = I$, so `Adjoint H = H`.
```csharp
H(q); // |0⟩ → |+⟩
ApplyToEach(H, qs); // H⊗n on qubit array
```

**S gate (phase) - `S`**
$$S = \begin{pmatrix}1 & 0\\0 & i\end{pmatrix}$$
Applies phase $i$ to $|1\rangle$. $S = T^2$. $S^\dagger$ applies phase $-i$.
```csharp
S(q);
Adjoint S(q); // S†: applies -i phase to |1⟩
```

**T gate ($\pi/8$ gate) - `T`**
$$T = \begin{pmatrix}1 & 0\\0 & e^{i\pi/4}\end{pmatrix}$$
Applies phase $e^{i\pi/4}$ to $|1\rangle$. $T^8 = I$. Together with $H$ & [[CNOT]], forms **universal gate set** for quantum computation. T-gate count is the standard measure of fault-tolerant circuit cost.
```csharp
T(q);
Adjoint T(q); // T†: applies e^{-iπ/4} phase to |1⟩
```

| Gate | [[Matrix]] diagonal / action                                  | Self-inverse?           |
| ---- | --------------------------------------------------------- | ----------------------- |
| X    | Flips $\|0\rangle\leftrightarrow\|1\rangle$               | Yes                     |
| Y    | $\|0\rangle\to i\|1\rangle$, $\|1\rangle\to -i\|0\rangle$ | Yes                     |
| Z    | $\|1\rangle\to -\|1\rangle$                               | Yes                     |
| H    | $\|0\rangle\to\|+\rangle$, $\|1\rangle\to\|-\rangle$      | Yes                     |
| S    | $\|1\rangle\to i\|1\rangle$                               | No ($S^\dagger \neq S$) |
| T    | $\|1\rangle\to e^{i\pi/4}\|1\rangle$                      | No ($T^\dagger \neq T$) |

All six are in `Std.Intrinsic` & support `Adj + Ctl`. See [[Rotation gates]] for parameterized variants $R_x, R_y, R_z, R_1$.
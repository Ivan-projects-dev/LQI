#Math #Q-Sharp #Python 
**Pauli-Z gate** (**phase flip gate**) leaves $|0\rangle$ unchanged & flips the sign of $|1\rangle$. It introduces **relative phase** between $2$ basis states without changing measurement probabilities - making it invisible to computational-basis measurements but detectable in the [[Hadamard]] basis.
$$Z = \begin{pmatrix}1 & 0\\0 & -1\end{pmatrix}$$
$$Z|0\rangle = |0\rangle \qquad Z|1\rangle = -|1\rangle$$
On superpositions: $Z(\alpha|0\rangle + \beta|1\rangle) = \alpha|0\rangle - \beta|1\rangle$.

Measuring after $Z$ alone gives the same outcome distribution as before - the $-1$ phase is **relative phase**, not a global phase, & is physically meaningful.

$Z^2 = I$ - self-inverse & Hermitian. `Adjoint Z = Z`.

$Z$ is diagonal in the computational basis, making it the cheapest gate to implement on most hardware (often **virtual gate** - just frame update in software, $0$ physical time).
$$Z = S^2 = T^4 \qquad Z = R_z(\pi) \cdot e^{i\pi/2} \qquad Z = HXH$$
[[Hadamard]] conjugation $HZH = X$ means: a $Z$ error after $H$ is equivalent to an $X$ error before. This is the basis of all stabilizer error correction - $Z$ errors in the $|{+}\rangle/|{-}\rangle$ basis look like $X$ errors.

| Gate | [[Matrix]] | Phase on $\|1\rangle$ |
|---|---|---|
| $T$ | $\begin{pmatrix}1&0\\\\0&e^{i\pi/4}\end{pmatrix}$ | $e^{i\pi/4}$ |
| $S = T^2$ | $\begin{pmatrix}1&0\\\\0&i\end{pmatrix}$ | $e^{i\pi/2}$ |
| $Z = T^4$ | $\begin{pmatrix}1&0\\\\0&-1\end{pmatrix}$ | $e^{i\pi}$ |
### Bloch sphere geometry
$Z$ is $180°$ rotation about the $\hat{z}$ axis. North & south poles are fixed; all equatorial states rotate $180°$$$Z|{+}\rangle = |{-}\rangle \qquad Z|{-}\rangle = |{+}\rangle$$

| Eigenvalue | Eigenstate |
|---|---|
| $+1$ | $\|0\rangle$ |
| $-1$ | $\|1\rangle$ |
Computational basis measurement is exactly $Z$-basis measurement.
### Phase kickback & Z errors
$Z$ is the gate that **appears** on control qubit due to phase kickback when the controlled operation is an $X$-type flip & the target is in $|{-}\rangle$. After the oracle applies $(-1)^{f(x)}$ to $|x\rangle$, what has happened physically is that $Z^{f(x)}$ was applied to the register - $Z$ error pattern encoding the function.

In [[Grover]]'s algorithm, the oracle precisely applies $Z$ to the marked solution state: $$Z|w\rangle|{-}\rangle \to -|w\rangle|{-}\rangle$$CZ gate:
`Controlled Z([ctrl], target)` or `CZ(q0, q1)` is **symmetric** - control & target are interchangeable:
$$CZ = \begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&1&0\\0&0&0&-1\end{pmatrix}$$
Only $|11\rangle$ picks up phase of $-1$. Widely used in superconducting hardware (native $2$-qubit gate alongside CX on some backends). CZ is its own inverse.
### Virtual Z gates
On many hardware platforms, $R_z(\theta)$ (and therefore $Z$, $S$, $T$) is implemented as **frame update**: the software rotates the reference frame for subsequent pulses rather than applying physical microwave pulse. This means:
- $0$ gate time, $0$ decoherence exposure
- Perfect fidelity in principle
- Gate appears in the circuit but costs nothing on physical hardware
IBM's transpiler exploits this via the **RZ-SX decomposition**: every single-qubit gate is decomposed into RZ (virtual) + SX (physical) gates.
$Z$ appears in:
- **Phase oracle construction**: marking solutions with a $-1$ phase
- **Pauli error channels**: $Z$ errors model dephasing noise
- **QFT**: $R_1(\theta)$ = diagonal phase gate, reduces to $Z$ at $\theta = \pi$
- **Hamiltonian simulation**: $ZZ$ coupling is the natural interaction for Ising models
- **Measurement**: $Z$-basis measurement is the default; measuring any other observable requires rotating to $Z$ basis first
```csharp
Z(q); // phase flip on |1⟩
CZ(q0, q1); // controlled-Z (symmetric)
Controlled Z([ctrl], target); // same as CZ
```
**Qiskit**
```python
qc.z(0) # Z gate
qc.cz(0, 1) # CZ gate (symmetric)
qc.s(0) # S = Z^(1/2)
qc.sdg(0) # S† = Z^(-1/2)
qc.t(0) # T = Z^(1/4)
qc.tdg(0) # T†
```
**PennyLane**
```python
qml.PauliZ(wires=0)
qml.CZ(wires=[0, 1])
```

| Property | Value |
|---|---|
| [[Matrix]] | $\begin{pmatrix}1 & 0\\\\0 & -1\end{pmatrix}$ |
| Unitary | Yes |
| Hermitian | Yes |
| Self-inverse | Yes ($Z^2 = I$) |
| Clifford gate | Yes |
| Bloch rotation | $180°$ about $\hat{z}$ |
| [[T gate]] count | $0$ |
| Implementation | Often virtual (frame update) |
| Q# name | `Z` |
| Qiskit name | `z` |
| [[PennyLane]] name | `qml.PauliZ` |

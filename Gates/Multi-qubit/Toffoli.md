#Math #Q-Sharp #Algorithm #Physics #Python 
**Toffoli gate** (CCNOT - doubly-controlled NOT) flips target qubit if & only if **both** control [[Qubits]] are $|1\rangle$. It is the quantum generalization of the classical AND gate & is **universal for reversible classical computation**
$$CCNOT: |c_1\, c_2\, t\rangle \mapsto |c_1\, c_2\,(t \oplus (c_1 \wedge c_2))\rangle$$
[[Matrix]] representation:
$$CCNOT = \begin{pmatrix}
1&0&0&0&0&0&0&0\\
0&1&0&0&0&0&0&0\\
0&0&1&0&0&0&0&0\\
0&0&0&1&0&0&0&0\\
0&0&0&0&1&0&0&0\\
0&0&0&0&0&1&0&0\\
0&0&0&0&0&0&0&1\\
0&0&0&0&0&0&1&0
\end{pmatrix}$$
$8\times8$ identity with the last $2$ rows swapped: only $|110\rangle \leftrightarrow |111\rangle$ is affected.

| Input $\|c_1 c_2 t\rangle$                               | Output         |
| -------------------------------------------------------- | -------------- |
| $\|000\rangle, \|001\rangle, \|010\rangle, \|011\rangle$ | unchanged      |
| $\|100\rangle, \|101\rangle$                             | unchanged      |
| $\|110\rangle$                                           | $\|111\rangle$ |
| $\|111\rangle$                                           | $\|110\rangle$ |
Only the $c_1 = c_2 = 1$ subspace is touched. $CCNOT^2 = I$ - self-inverse.
### Classical universality
Any classical Bool circuit can be made reversible using Toffoli & NOT gates:
- **AND**: $CCNOT(a, b, |0\rangle)$ computes $a \wedge b$ in the target, leaving $a$ & $b$ intact
- **NOT**: $X(t)$ - the standard [[Pauli-X]] gate
- **FANOUT**: copy a bit using $[[CNOT]](a, |0\rangle)$
Together these reproduce all bool funcs reversibly. This makes CCNOT the foundation of [[Oracle]] construction in [[Grover]], [[Shor]], & all other algorithms that need to evaluate a classical func on quantum superposition.
### Quantum universality
CCNOT + H is universal for quantum computation. This is because:
- CCNOT alone is universal for classical reversible computation
- H creates superposition & interference
- Together they span the full unitary group $U(2^n)$ (up to global phase)

In practice standard universal set $\{H, T, [[CNOT]]\}$ is preferred for fault-tolerant analysis because T-gate count drives physical resource costs.

**Relative phase Toffoli** achieves same unitary on the $|110\rangle \to |111\rangle$ transition while picking up phases on other states. These variants require only **$3$ T gates** but are **not self-inverse** & can only be used in contexts where the controls are guaranteed to return to $|0\rangle$ by the end (within/apply pattern). The savings are significant in large oracles.

Every [[Oracle]] that evaluates bool func uses Toffoli gates as the core building block:
```csharp
// Marking |111⟩ among 3-qubit states
Controlled X([q0, q1], q2); // CCNOT: flip q2 iff q0=q1=1
```
For arbitrary bit patterns, $X$ gates wrap the CCNOT:
```csharp
within {
    X(q0); // flip qubit 0 (targeting |0⟩ in q0)
}
apply {
    CCNOT(q0, q1, ancilla); // fires when original q0=0, q1=1
}
```
Multi-controlled $X$ with $>$ controls decomposes into ladder of Toffoli gates using borrowed [[Ancilla]]:
$$C^n X: O(n) \text{ Toffoli gates, } O(n) \text{ borrowed [[Ancilla]]}$$
Q# provides `Microsoft.Quantum.Canon.ApplyAnd` which implements an optimized Toffoli that consumes fewer T gates by using measurement-based uncomputation. It only works within a `within/apply` block (the apply half) & **cannot** be used standalone:
```csharp
within {
    ApplyAnd(c0, c1, ancilla); // 4 T-gates instead of 7
}
apply {
    X(target); // conditioned on ancilla
}
// ancilla automatically uncomputed to |0⟩ using MeasureAndReset
```

| Operation            | Q# syntax                     | T-gate cost |
| -------------------- | ----------------------------- | ----------- |
| Toffoli (2 controls) | `CCNOT(c0, c1, t)`            | $7$         |
| 3-controlled X       | `Controlled X([c0,c1,c2], t)` | $~20$       |
| $n$-controlled X     | `Controlled X(ctrls, t)`      | $O(n)$      |
Q# auto-generates multi-controlled decompositions using borrowed [[Ancilla]] from the surrounding scope.
```csharp
CCNOT(ctrl1, ctrl2, target); // built-in Toffoli
Controlled X([ctrl1, ctrl2], target); // equivalent

// n-controlled:
Controlled X([c0, c1, c2, c3], target); // 4-controlled X

// Optimized AND (within/apply only):
use anc = Qubit();
within {
    ApplyAnd(c0, c1, anc);
}
apply {
    CNOT(anc, target);
}
```

**Qiskit**
```python
qc.ccx(0, 1, 2) # Toffoli: controls=0,1, target=2
qc.mcx([0,1,2], 3) # multi-controlled X: controls=0,1,2, target=3
```

**[[PennyLane]]**
```python
qml.Toffoli(wires=[0, 1, 2]) # controls=0,1, target=2
qml.MultiControlledX(wires=[0,1,2,3]) # 3-controlled X
```

| Property               | Value                          |
| ---------------------- | ------------------------------ |
| [[Qubits]]             | $3$ ($2$ control $+ 1$ target) |
| [[Matrix]] size        | $8 \times 8$                   |
| Self-inverse           | Yes ($CCNOT^2 = I$)            |
| Clifford gate          | No                             |
| [[T gate]] cost            | $7$ (optimal)                  |
| Classical universality | Yes (with NOT)                 |
| Quantum universality   | Yes (with $H$)                 |
| Q# name                | `CCNOT`                        |
| Qiskit name            | `ccx`                          |
| [[PennyLane]] name     | `qml.Toffoli`                  |

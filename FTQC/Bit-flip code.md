#Quantum #[[FTQC]]
Simplest quantum error correcting code. Encodes $1$ **logical qubit** into 3 physical [[Qubits]]; protects against a single **bit-flip** ($X$) error on any $1$ physical qubit. Quantum analogue of the classical repetition code (but state cannot be cloned - encoding uses entanglement instead).

**Encoding**: $|\bar\psi\rangle = \alpha|000\rangle + \beta|111\rangle$ from $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$:
```
q[0]: |ψ⟩ ──●──●──
q[1]: |0⟩ ──⊕──|──
q[2]: |0⟩ ──|──⊕──
```
Two CNOTs (control = q[0]): [[CNOT]](q[0], q[1]); [[CNOT]](q[0], q[2]).

**Parity measurement** (syndrome extraction): XOR pairs of [[Qubits]] without collapsing the logical state. Implemented by [[CNOT]]-ing each qubit into a fresh [[Ancilla]] & measuring the [[Ancilla]]:
- $p_{01} = q_0\oplus q_1$ & $p_{12} = q_1\oplus q_2$

| $p_{01}$ | $p_{12}$ | Error |
|----------|----------|-------|
| 0 | 0 | None |
| 1 | 0 | X on q[0] |
| 1 | 1 | X on q[1] |
| 0 | 1 | X on q[2] |

[[Ancilla]] [[Qubits]] are reset after each syndrome extraction & reused.

**Error correction**: apply $X$ to the flagged qubit. After correction, state returns to $|\bar\psi\rangle$.

**Logical gates**:
- $\bar{X}$ (logical NOT): apply $X$ to all three physical [[Qubits]]. $\alpha|000\rangle+\beta|111\rangle \to \beta|000\rangle+\alpha|111\rangle$.
- $\bar{Z}$ (logical phase flip): apply $Z$ to any $1$ physical qubit (e.g., q[0]). $\alpha|000\rangle+\beta|111\rangle \to \alpha|000\rangle-\beta|111\rangle$.

**Limitations**:
- Does **not** protect against $Z$ errors: a $Z$ on any qubit maps $|\bar\psi\rangle$ to a valid codeword for $\alpha|0\rangle-\beta|1\rangle$ - undetectable.
- Does **not** protect against two simultaneous $X$ errors (misidentified as a single error on the third qubit).
- Full [[FTQC]] requires codes protecting against arbitrary single-qubit errors (e.g., Shor 9-qubit code, Steane 7-qubit code).

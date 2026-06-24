#FTQC #ErrorCorrection
The **phase-flip code** is the 3-qubit code that corrects $Z$ (phase-flip) errors. It is the dual of the [[Bit-flip code]], which corrects $X$ (bit-flip) errors. Together, they are concatenated in the [[Shor]] 9-qubit code to correct arbitrary single-qubit errors.

### Encoding
$$|\bar{0}\rangle = |{+}{+}{+}\rangle = \frac{(|0\rangle+|1\rangle)^{\otimes 3}}{2\sqrt{2}}$$
$$|\bar{1}\rangle = |{-}{-}{-}\rangle = \frac{(|0\rangle-|1\rangle)^{\otimes 3}}{2\sqrt{2}}$$

**Encoding circuit**: CNOT to spread the logical qubit across 3, then H on each:
```
|ψ⟩ ──●──●──H──
      |  |
|0⟩ ──X──┼──H──
         |
|0⟩ ─────X──H──
```
1. CNOT from qubit 0 to 1 and 2 (copies $|0\rangle$/$|1\rangle$) — same as bit-flip encoding
2. H on each qubit — maps to $X$ basis, converting $Z$ errors into detectable $X$ errors

### Why it works
A $Z$ error on qubit $i$ flips the sign of that qubit's $|{+}\rangle$/$|{-}\rangle$:
$$Z|{+}\rangle = |{-}\rangle \qquad Z|{-}\rangle = |{+}\rangle$$

Measuring in the $X$ basis (i.e., measuring $XX$ stabilizers) reveals which qubit flipped.

### Syndrome extraction ($X$-basis stabilizers)
| Stabilizer | Measures |
|---|---|
| $X_1 X_2$ | Whether qubits 1 and 2 have same sign |
| $X_2 X_3$ | Whether qubits 2 and 3 have same sign |

Syndrome table:
| $X_1X_2$ | $X_2X_3$ | Error |
|---|---|---|
| $+1$ | $+1$ | None |
| $-1$ | $+1$ | $Z_1$ |
| $-1$ | $-1$ | $Z_2$ |
| $+1$ | $-1$ | $Z_3$ |

Correction: apply $Z$ to the identified qubit.

### Limitations
Corrects single $Z$ errors only. Cannot correct $X$ (bit-flip) errors — for that you need the [[Bit-flip code]]. Cannot correct $Y = iXZ$ errors (which have both components).

### Shor code: combining both
Shor's 9-qubit code $[[9,1,3]]$ concatenates:
1. Outer **phase-flip code** (3 logical blocks in $X$ basis)
2. Inner **bit-flip code** (each logical qubit encoded in 3 physical qubits)

This corrects any single-qubit error ($X$, $Z$, or $Y$) and was the first quantum error-correcting code. Modern surface codes supersede it for practical use but it remains the canonical pedagogical example.

Source: [Shor — "Scheme for reducing decoherence in quantum computer memory" (1995)](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.52.R2493)

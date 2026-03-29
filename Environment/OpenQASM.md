#Quantum #Q-Sharp #Programming
**OpenQASM (Open Quantum Assembly Language)** is imperative prog lang for describing quantum circuits. Originally dev at IBM ($2017$), now maintained as open standard. Used by Quantinuum on [[Azure Quantum]] & supported by the QDK compiler.

**OpenQASM $2.0$** - original, widely adopted version. C-like syntax for defining gate operations & measurements. Limited to purely gate-based, shot-sequential circuits. No real-time classical control flow.
```bash
OPENQASM 2.0;
include "qelib1.inc";
qreg q[2];
creg c[2];
h q[0];
cx q[0], q[1];
measure q -> c;
```

**OpenQASM $3.0$** major extension published $2021$ (**arXiv:2104.14722**). Adds:
- **Classical control flow** - `if`, `for`, `while`, `switch` on classical registers
- **Real-time classical compute** - inline classical arithmetic & bit manipulation
- **Timing & pulse control** - `delay`, `stretch`, `box` for precise scheduling; `cal` blocks for pulse-level calibration
- **Parameterized circuits** - I/O modifiers let circuits accept parameters at runtime (compile once, run many times)
- **External function calls** - `extern` keyword for calling classical functions from within the circuit
- **Subroutines** -`def` keyword to define reusable gate & circuit fragments
- **Improved type system** - `bit`, `int`, `uint`, `float`, `angle`, `duration`, `stretch`

| Context                | Version               | Notes                                            |
| ---------------------- | --------------------- | ------------------------------------------------ |
| Quantinuum $H2$ target | OpenQASM $2.0$ native | Primary input format for $H2$                    |
| QDK compiler           | OpenQASM $3.0$        | Compiles to QIR; use `qsharp.openqasm.compile()` |
| Rigetti                | Quil (not QASM)       | Different assembly standard                      |

In the QDK, compile OpenQASM 3.0 to QIR using:
```python
import qsharp.openqasm as openqasm

qir_bitcode = openqasm.compile("qubit q; bit c; h q; c = measure q;")
```

OpenQASM $3.0$ & QIR serve complementary roles:
- **OpenQASM** - human-readable circuit description language (text format)
- **QIR** - binary compilation target (LLVM bitcode) for hardware execution

QDK accepts OpenQASM $3$ source, compiles to QIR, & submits to any provider backend — bridging the two formats.

## Sources
- [OpenQASM 3.0 specification](https://openqasm.com/versions/3.0/intro.html)
- [OpenQASM 3 paper (arXiv:2104.14722)](https://arxiv.org/abs/2104.14722)
- [Run OpenQASM in QDK](https://learn.microsoft.com/en-us/azure/quantum/qdk-openqasm-integration)
- [Quantinuum provider on Azure Quantum](https://learn.microsoft.com/en-us/azure/quantum/provider-quantinuum)

#Quantum #Cloud #Platform
Quick-reference comparison of all available quantum platforms.

## Purpose Map

| Goal | Platform | Entry point |
|---|---|---|
| Learn quantum basics | [[IBM Quantum]] | `learning.quantum.ibm.com` |
| Structured algorithm exercises | [[Azure Quantum]] ([[Quantum Katas]]) | VS Code + QDK extension |
| FTQC resource analysis | [[Azure Quantum]] ([[Quantum resource estimator]]) | Azure portal |
| Quantum ML / variational | [[Xanadu PennyLane]] | `pip install pennylane` |
| Combinatorial optimization | [[D-Wave Leap]] | `cloud.dwavesys.com` |
| Multi-hardware comparison | [[Amazon Braket]] | AWS console |
| 34+ device access | [[qBraid]] | `qbraid.com` |

## Hardware Technology Comparison

| Platform | Qubit type | Gate speed | Coherence | Connected? |
|---|---|---|---|---|
| IBM Quantum | Superconducting | ~$50$ ns | ~$100$ μs | Nearest-neighbor |
| IonQ (via Braket) | Trapped ion | ~$100$ μs | ~seconds | All-to-all |
| Rigetti (via Braket) | Superconducting | ~$50$ ns | ~$100$ μs | Nearest-neighbor |
| QuEra (via Braket) | Neutral atom (analog) | N/A | ~seconds | Reconfigurable |
| D-Wave | Flux qubit (annealer) | Sub-ms anneal | N/A | Pegasus graph |

## Free Tier Summary

| Platform | Free hardware access | Free simulation |
|---|---|---|
| [[IBM Quantum]] | $10$ min QPU/month | Unlimited (AerSimulator) |
| [[Azure Quantum]] | Limited credits | Unlimited local Q# |
| [[Amazon Braket]] | None | $1$ hr/month managed (12 mo) + unlimited local |
| [[D-Wave Leap]] | Limited QPU time | Unlimited local (SimulatedAnnealing) |
| PennyLane | Via connected backends | Unlimited (`default.qubit`) |
| [[qBraid]] | Limited | Unlimited (local devices) |

## Paradigm Comparison

**Gate-model** (IBM, Azure, Braket, PennyLane): prepare [[Qubits]] → apply gates → measure → analyze distribution.

**[[Quantum annealing]]** (D-Wave): define energy function ([[QUBO]]/Ising) → annealer finds minimum energy configuration.

**Analog simulation** (QuEra Aquila on Braket): define Hamiltonian via atom positions & laser pulses → observe evolved state.

## Beginner Guides

- [[IBM Quantum Basics]]
- [[Azure Quantum Basics]]
- [[PennyLane Basics]]
- [[Amazon Braket Basics]]
- [[D-Wave Leap Basics]]
- [[Quantum Computing - What It Is]]
- [[Quantum Platforms - Common Surprises]]
- [[Quantum Platforms Learning Path]]

## Sources
- [IBM Quantum Platform](https://quantum.cloud.ibm.com)
- [Azure Quantum](https://azure.microsoft.com/en-us/products/quantum)
- [Amazon Braket pricing](https://aws.amazon.com/braket/pricing/)
- [PennyLane](https://pennylane.ai)
- [D-Wave Leap](https://cloud.dwavesys.com)
- [qBraid](https://qbraid.com)

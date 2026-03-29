#Quantum #Cloud #Hardware
[[Azure Quantum]] gives access to $3rd$-party **quantum hardware providers**, each offering real QPUs & simulators through unified API. Providers are added to [[Azure Quantum Workspace]] and billed separately according to their own pricing plans.

**IonQ** - trapped-ion computers (Aria $1: 25$ [[Qubits]]; Forte $1$ & Forte Enterprise - $1: 36$ [[Qubits]]) $+$ GPU-accelerated simulator up to $29$ [[Qubits]]. Billing is per gate-shot (single-qubit: $~0.00022$ USD, $2$-qubit: $~0.00098$ USD) with $1–5$ USD min per program run.

**Quantinuum** - trapped-ion System Model $H2$ series. Billing uses **Hardware Quantum Credits (HQCs)**, charged per operation $×$ shots. Offers Standard & Premium plans via Azure.

**Rigetti** - superconducting QPUs. Billing is by **job exec time** only (no per-shot or per-gate charge). Includes a free open-source QVM simulator.

**PASQAL** - neutral-atom processor (Fresnel, $100$ [[Qubits]]) $+$ EMU-TN [[Tensor]]-network emulator. Billing is by exec time.

All providers support Q#, Qiskit, or Cirq circuits submitted through [[Azure Quantum Jobs|Azure Quantum jobs]]. Availability by region may vary.
## Provider Details

**IonQ** - targets: `ionq.simulator` (up to 29 [[Qubits]], GPU-accelerated), `ionq.qpu.aria-1` (25 [[Qubits]], ~20 algorithmic [[Qubits]]), `ionq.qpu.forte-1` & `ionq.qpu.forte-enterprise-1` (36 [[Qubits]]). Billing uses **[[Azure Quantum]] Tokens (AQT)** - includes a minimum price per execution (≈$12.42 with [[Error Mitigation]] off, ≈$97.50 with it on), plus per-gate-shot charges. Supports IonQ native JSON format & QIR.

**Quantinuum** - targets: `quantinuum.qpu.h2-1` (System Model H2, 56 [[Qubits]], all-to-all connectivity, mid-circuit measurement). Note: H1-1 hardware was **retired October 15, 2025**. Billing uses **H-System Quantum Credits (HQT)** = operations × shots. Standard & Premium subscription plans available; Pay-as-you-Go via Quantinuum sales. Supports [[OpenQASM]] 2.0 & native Quantinuum machine code.

**Rigetti** - targets: `rigetti.sim.qvm` (free open-source QVM), `rigetti.qpu.ankaa-9q-3` & newer Ankaa series. Billing per **QPU exec second** - no per-shot or per-gate charge. Input format: **Quil** (`rigetti.quil.v1`). Ankaa series uses a 2D square lattice layout with tunable couplers for improved 2-qubit gate fidelity.

**PASQAL** - targets: `pasqal.sim.emu-tn` (EMU-TN [[Tensor]]-network emulator), `pasqal.qpu.fresnel` (100-qubit neutral-atom Fresnel QPU). Billing per exec time on both QPU & emulator. Input uses **Pulser** sequences (pulse-level programming). Neutral-atom approach allows flexible 2D/3D qubit array layouts & long coherence times.

All providers can be added/removed from a workspace after creation via the Azure portal or CLI. Provider availability varies by Azure region.

## Sources
- [Provider & target list](https://learn.microsoft.com/en-us/azure/quantum/qc-target-list)
- [IonQ provider](https://learn.microsoft.com/en-us/azure/quantum/provider-ionq)
- [Quantinuum provider](https://learn.microsoft.com/en-us/azure/quantum/provider-quantinuum)
- [Rigetti provider](https://learn.microsoft.com/en-us/azure/quantum/provider-rigetti)
- [PASQAL provider](https://learn.microsoft.com/en-us/azure/quantum/provider-pasqal)
- [Global provider availability](https://learn.microsoft.com/en-us/azure/quantum/provider-global-availability)

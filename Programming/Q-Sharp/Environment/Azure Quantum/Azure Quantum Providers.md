#Quantum #Cloud #Hardware
[[Azure Quantum]] gives access to $3rd$-party **quantum hardware providers**, each offering real QPUs & simulators through unified API. Providers are added to [[Azure Quantum Workspace]] and billed separately according to their own pricing plans.

**IonQ** - trapped-ion computers (Aria $1: 25$ [[Qubits]]; Forte $1$ & Forte Enterprise - $1: 36$ [[Qubits]]) $+$ GPU-accelerated simulator up to $29$ [[Qubits]]. Billing is per gate-shot (single-qubit: $~0.00022$ USD, $2$-qubit: $~0.00098$ USD) with $1–5$ USD min per program run.

**Quantinuum** - trapped-ion System Model $H2$ series. Billing uses **Hardware Quantum Credits (HQCs)**, charged per operation $×$ shots. Offers Standard & Premium plans via Azure.

**Rigetti** - superconducting QPUs. Billing is by **job exec time** only (no per-shot or per-gate charge). Includes a free open-source QVM simulator.

**PASQAL** - neutral-atom processor (Fresnel, $100$ [[Qubits]]) $+$ EMU-TN [[Tensor]]-network emulator. Billing is by exec time.

All providers support Q#, Qiskit, or Cirq circuits submitted through [[Azure Quantum Jobs|Azure Quantum jobs]]. Availability by region may vary.
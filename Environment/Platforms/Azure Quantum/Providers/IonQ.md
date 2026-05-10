#SoftDev 
**IonQ** - trapped-ion computers (Aria $1: 25$ [[Qubits]]; Forte $1$ & Forte Enterprise - $1: 36$ [[Qubits]]) $+$ GPU-accelerated simulator up to $29$ [[Qubits]]. Billing is per gate-shot (different rates for single-qubit vs $2$-qubit gates) with a minimum fee per program run.

## Targets
`ionq.simulator` (up to $29$ [[Qubits]], GPU-accelerated), `ionq.qpu.aria-1` (25 [[Qubits]], ~20 algorithmic [[Qubits]]), `ionq.qpu.forte-1` & `ionq.qpu.forte-enterprise-1` (36 [[Qubits]]). 
Billing uses **[[Azure Quantum]] Tokens (AQT)** - includes a minimum fee per execution (higher with [[Error Mitigation]] enabled), plus per-gate-shot charges. 
Supports IonQ native JSON format & QIR.

Pricing model: minimum fee per execution + per-gate-shot charges. [[Error Mitigation]] significantly increases the minimum per-execution cost. Monthly subscription plans available at discounted rates for heavy users.

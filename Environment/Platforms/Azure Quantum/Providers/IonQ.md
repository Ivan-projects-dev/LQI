#SoftDev 
**IonQ** - trapped-ion computers (Aria $1: 25$ [[Qubits]]; Forte $1$ & Forte Enterprise - $1: 36$ [[Qubits]]) $+$ GPU-accelerated simulator up to $29$ [[Qubits]]. Billing is per gate-shot (single-qubit: $~0.00022$ USD, $2$-qubit: $~0.00098$ USD) with $1-5$ USD min per program run.

## Targets
`ionq.simulator` (up to $29$ [[Qubits]], GPU-accelerated), `ionq.qpu.aria-1` (25 [[Qubits]], ~20 algorithmic [[Qubits]]), `ionq.qpu.forte-1` & `ionq.qpu.forte-enterprise-1` (36 [[Qubits]]). 
Billing uses **[[Azure Quantum]] Tokens (AQT)** - includes min price/execution (≈$12.42 with [[Error Mitigation]] off, ≈$97.50 with it on), plus per-gate-shot charges. 
Supports IonQ native JSON format & QIR.

Pricing formula: min fee per execution + per-gate-shot charges. Min fee ≈$12.42 ([[Error Mitigation]] off) or ≈$97.50 ([[Error Mitigation]] on). Single-qubit gate: ≈$0.00022/shot; 2-qubit gate: ≈$0.00098/shot. Monthly subscription plans available at discounted rates for heavy users.


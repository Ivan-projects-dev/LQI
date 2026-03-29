#Quantum #[[FTQC]] #QEC
The **surface code** is the most widely studied topological quantum error correcting code. It is the default QEC scheme in the [[QRE]] and the leading candidate for fault-tolerant quantum computing on near-term hardware.

## Structure

[[Qubits]] are arranged on a 2D lattice of size $d × d$ (code **distance** $d$). The lattice contains two types of physical [[Qubits]]:
- **Data [[Qubits]]** - store the encoded logical information
- **Measure [[Qubits]]** ([[Ancilla]]) - used to extract error syndromes without disturbing the logical state

Each logical qubit of distance $d$ requires $d^2$ data [[Qubits]] + $(d^2-1)$ measure [[Qubits]] ≈ $2d^2$ physical [[Qubits]] total.

Two types of stabilizer measurements:
- **X-type (star) stabilizers** - detect Z (phase-flip) errors
- **Z-type (plaquette) stabilizers** - detect X (bit-flip) errors

Since X and Z errors are detected independently, the surface code protects against arbitrary single-qubit errors (any combination of X, Y, Z).

## Error Threshold

The surface code has a fault-tolerance **threshold** of ≈1% physical gate error rate. Below this threshold, increasing $d$ exponentially suppresses the logical error rate:

$$p_L \approx \left(\frac{p}{p_{th}}\right)^{\lfloor (d+1)/2 \rfloor}$$

where $p$ = physical error rate, $p_{th}$ ≈ 1%, and $p_L$ = logical error rate. Google's 2024 Willow chip demonstrated error suppression factor $\Lambda$ = 2.14 per step of code distance increase — first clear demonstration below the surface code threshold.

## Code Distance & Physical Qubit Cost

| Code distance $d$ | Physical [[Qubits]] per logical qubit | Target logical error rate |
|---|---|---|
| 5 | ~50 | ~$10^{-4}$ |
| 7 | ~100 | ~$10^{-6}$ |
| 15 | ~450 | ~$10^{-10}$ |
| 27 | ~1,457 | ~$10^{-15}$ |

The [[QRE]] selects $d$ automatically based on your error budget and physical error rate assumptions.

## Logical Gates

- **Transversal Clifford gates** (H, S, [[CNOT]]) — applied qubit-by-qubit across code blocks; inherently fault-tolerant
- **T-gate** — NOT transversal in surface code; must be implemented via **magic state distillation** (see [[QRE]] T-factories)
- **Logical measurement** — measure all data [[Qubits]]; used for lattice surgery-based [[CNOT]]

**Lattice surgery** is the preferred method for performing logical gates between surface code patches — two patches are merged & split by temporarily joining their boundaries, avoiding the overhead of transversal 2-qubit gates.

## Rotated vs. Unrotated Surface Code

- **Rotated** (tilted) — $d^2$ data + $(d^2-1)$ measure = fewer total [[Qubits]] for same distance; preferred in practice
- **Unrotated** — symmetric, easier to analyze but less qubit-efficient

## Comparison to Other Codes

| Code | Physical/logical [[Qubits]] | Threshold | Practical? |
|---|---|---|---|
| [[Bit-flip code]] | 3 | None for Z errors | No (NISQ only) |
| Steane [7,1,3] | 7 | ~$10^{-4}$ | Limited |
| **Surface code** | ~$2d^2$ | ~1% | Yes |
| Floquet code | similar | similar | Emerging |

## Sources
- [Quantum error correction below the surface code threshold (Nature, 2024)](https://www.nature.com/articles/s41586-024-08449-y)
- [arXiv: QEC below threshold (2408.13687)](https://arxiv.org/abs/2408.13687)
- [Google Research: suppressing errors by scaling a surface code logical qubit](https://research.google/blog/suppressing-quantum-errors-by-scaling-a-surface-code-logical-qubit/)
- [Resource Estimator QEC schemes](https://learn.microsoft.com/en-us/azure/quantum/overview-resources-estimator)

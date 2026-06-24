#Math #Algorithm 
**Surface code** is the most widely studied topological quantum error correcting code. It is the default QEC scheme in the [[QRE]] & the leading candidate for fault-tolerant quantum computing on near-term hardware.

[[Qubits]] are arranged on $2D$ lattice of size $d × d$ (code **distance** $d$). Lattice contains $2$ types of physical [[Qubits]]:
- **Data [[Qubits]]** - store the encoded logical info
- **Measure [[Qubits]]** ([[Ancilla]]) - used to extract error syndromes without disturbing the logical state

Each logical qubit of distance $d$ requires $d^2$ data [[Qubits]] + $(d^2-1)$ measure [[Qubits]] ≈ $2d^2$ physical [[Qubits]] total. $2$ types of stabilizer measurements:
- **$X$-type (star) stabilizers** - detect $Z$ (phase-flip) errors
- **$Z$-type (plaquette) stabilizers** - detect $X$ (bit-flip) errors

Since $X$ & $Z$ errors are detected independently, the surface code protects against arbitrary single-qubit errors (any combination of $X, Y, Z$).

**Error Threshold**: surface code has fault-tolerance **threshold** of $≈1$% physical gate error rate. Below this threshold, increasing $d$ exponentially suppresses the logical error rate:
$$p_L \approx \left(\frac{p}{p_{th}}\right)^{\lfloor (d+1)/2 \rfloor}$$
where $p$ = physical error rate, $p_{th} ≈ 1$%, & $p_L =$ logical error rate. Google's $2024$ Willow chip demonstrated error suppression factor $\Lambda = 2.14$ per code distance increment (Google Quantum AI, Nature 2024).

[[QRE]] selects $d$ automatically based on your error budget & physical error rate assumptions.
### Logical Gates
- **Transversal Clifford gates** ($H, S$, [[CNOT]]) - applied qubit-by-qubit across code blocks; inherently fault-tolerant
- **[[T gate]]** - NOT transversal in surface code; must be implemented via **magic state distillation** (see [[QRE]] T-factories)
- **Logical measurement** - measure all data [[Qubits]]; used for lattice surgery-based [[CNOT]]

**Lattice surgery** is the preferred method for performing logical gates between surface code patches - $2$ patches are merged & split by temporarily joining their boundaries, avoiding the overhead of transversal $2$-qubit gates.
- **Rotated** (tilted) - $d^2$ data + $(d^2-1)$ measure = fewer total [[Qubits]] for same distance; preferred in practice
- **Unrotated** - symmetric, easier to analyze but $<$ qubit-efficient

| Code              | Physical/logical [[Qubits]] | Threshold         | Practical?     |
| ----------------- | --------------------------- | ----------------- | -------------- |
| [[Bit-flip code]] | $3$                         | None for Z errors | No (NISQ only) |
| Steane $[7,1,3]$  | $7$                         | $~10^{-4}$        | Limited        |
| **Surface code**  | $~2d^2$                     | $~1$%             | Yes            |
| Floquet code      | similar                     | similar           | Emerging       |

Source: [Fowler et al. 2012 — Surface codes (arXiv)](https://arxiv.org/abs/1208.0928) [Google Quantum AI — Willow chip Nature 2024](https://www.nature.com/articles/s41586-024-08449-y) [Surface code — MS Learn](https://learn.microsoft.com/en-us/azure/quantum/concepts-the-code-space)

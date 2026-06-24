#FTQC #ErrorCorrection
A **logical qubit** is a fault-tolerant qubit encoded across many **physical qubits** using a quantum error-correcting code. While physical qubits suffer hardware noise, logical qubits can maintain coherence as long as the physical error rate is below a threshold.

### Physical vs logical
| | Physical qubit | Logical qubit |
|---|---|---|
| Implementation | Superconducting, trapped ion, etc. | $n$ physical qubits + stabilizer code |
| Error rate | $p \sim 10^{-3}$–$10^{-2}$ | $p_L \ll p$ (suppressed exponentially in $d$) |
| Gate set | Native (Rx, Ry, CNOT, …) | Clifford (transversal) + T (via [[Magic state distillation]]) |
| Coherence | Limited by hardware | Extended by active error correction |
| 2025 examples | 10–1000 μs | Seconds (projected at $d \gtrsim 25$) |

### Code notation: $[[n, k, d]]$
- $n$: physical qubits per logical qubit
- $k$: logical qubits encoded
- $d$: code distance — minimum weight of undetectable error

Examples:
| Code | $[[n,k,d]]$ | Physical/logical |
|---|---|---|
| Bit-flip (3-qubit) | $[[3,1,3]]$ | 3 |
| [[Bit-flip code\|Phase-flip (3-qubit)]] | $[[3,1,3]]$ | 3 |
| Steane | $[[7,1,3]]$ | 7 |
| [[Surface Code]] (distance $d$) | $[[d^2,1,d]]$ | $d^2$ |
| Surface code at $d=17$ | $[[289,1,17]]$ | 289 |

### Logical error rate
For the surface code, the logical error rate per cycle suppresses as:
$$p_L \approx A \left(\frac{p}{p_{\text{th}}}\right)^{(d+1)/2}$$
where $p_{\text{th}} \approx 1\%$ is the fault-tolerance threshold and $A \sim 0.1$. Increasing $d$ by 2 roughly squares the suppression.

### Transversal Clifford gates
The surface code supports **transversal** H, S, CNOT — applying the physical gate on each qubit in the block implements the logical gate. This is fault-tolerant because a single physical error cannot spread to uncorrectable weight.

The T gate is **not transversal** → requires [[Magic state distillation]].

### Fault-tolerant threshold
Below $p_{\text{th}} \approx 1\%$ (for surface code with circuit-level noise), increasing $d$ always reduces $p_L$. Above threshold, larger codes make things worse.

State-of-the-art (2025):
- IBM: $p \approx 0.1\%$–$0.3\%$ for 2Q gates → well below threshold
- Google Willow (2024): demonstrated $p_L$ decreasing with $d$ up to $d=7$, $\approx 300$ physical qubits per logical qubit

### Physical qubit budget
At $d=17$–$25$ (targeting $p_L \sim 10^{-10}$ for algorithmic reliability):
- Surface code: $d^2 \approx 289$–$625$ physical qubits per logical qubit
- Plus [[Magic state distillation]] factories: adds 50–90% overhead

Early FTQC algorithms (VQE, QPE for chemistry) require 100–10,000 logical qubits → $10^5$–$10^7$ physical qubits.

Source: [Fowler et al. — "Surface codes: Towards practical large-scale quantum computation" (2012)](https://arxiv.org/abs/1208.0928) [Google Quantum AI — "Quantum error correction below the surface code threshold" (2024)](https://arxiv.org/abs/2408.13687)

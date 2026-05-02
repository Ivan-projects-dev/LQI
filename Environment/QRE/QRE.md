#SoftDev #Q-Sharp #Python #Algorithm 
**Microsoft Quantum resource estimator** allows to assess architectural decisions, compare qubit tech, & determine the resources needed to execute given quantum algorithm. Can choose from pre-defined fault-tolerant protocols & specify assumptions of the underlying physical qubit model. Free of charge & doesn't require Azure account.

**[[Azure Quantum]] service (batched)** - submit multiple param configurations as single job to get the full Pareto frontier in $1$ call.

| Field              | Description                                                                            |
| ------------------ | -------------------------------------------------------------------------------------- |
| `physicalQubits`   | Total physical [[Qubits]] needed (algorithm + $T$-factories)                           |
| `logicalQubits`    | Logical [[Qubits]] used by the algorithm                                               |
| `logicalDepth`     | num of logical cycles                                                                  |
| `codeDistance`     | Surface/floquet code distance $d$ (determines error suppression)                       |
| `tStates`          | Total $T$-states consumed by the algorithm                                             |
| `tFactories`       | Num of $T$-factory copies running in parallel                                          |
| `tFactoryFraction` | Fraction of total [[Qubits]] used by $T$-factories                                     |
| `runtime`          | Estimated wall-clock runtime                                                           |
| `rQOPS`            | Reliable quantum operations per second of the target                                   |
| `errorBudget`      | Allowed failure probability (split across logical errors, $T$-distillation, rotations) |
$2$ built-in QEC schemes:
- **[[Surface Code]]** - $2D$ lattice, each logical qubit requires $2d^2$ physical [[Qubits]], cycle time $∝ d$
- **Floquet code** - honeycomb-based, $>$ efficient use of connectivity, $<$ overhead in some regimes

Configurable qubit params (set `qubitParams` in the estimator job):
- `qubit_gate_ns_e3` - gate time $≈1 ns$, error rate ≈$10^{-3}$ (superconducting [[Qubits]])
- `qubit_gate_us_e3` - gate time $≈1 µs$, error rate ≈$10^{-3}$ (trapped ion [[Qubits]])
- Custom: specify `tGateTime`, `tGateErrorRate`, `twoQubitGateTime`, etc. directly
### Sources
- [Introduction to the Resource Estimator](https://learn.microsoft.com/en-us/azure/quantum/intro-to-resource-estimation)
- [Resource Estimator target parameters](https://learn.microsoft.com/en-us/azure/quantum/overview-resources-estimator)
- [Quickstart: use the Resource Estimator](https://learn.microsoft.com/en-us/azure/quantum/quickstart-microsoft-resources-estimator)
- [Space-time tradeoffs blog](https://quantum.microsoft.com/en-us/insights/blogs/resource-estimation/exploring-space-time-tradeoffs-with-azure-quantum-resource-estimator)
- [arXiv paper: using QRE for FTQC assessment](https://arxiv.org/abs/2311.05801)

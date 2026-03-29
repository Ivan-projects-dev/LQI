#Q-Sharp #Quantum 
Before running on real quantum hardware, you need to figure out if your program can run on existing hardware, & how many resources it'll consume.

**Microsoft Quantum resource estimator** allows to assess architectural decisions, compare qubit technologies, & determine the resources needed to execute a given quantum algorithm. You can choose from pre-defined fault-tolerant protocols & specify assumptions of the underlying physical qubit model. Free of charge & doesn't require Azure account.

**VS Code** - open any `.qs` file, right-click → *Estimate Resources*. Results appear in a side-panel table.

**Python (local)** - no Azure account needed:
```python
import qsharp
result = qsharp.estimate("MyOperation()")
print(result) # dict with all output fields
print(result.diagram)  # ASCII space-time diagram
```

**[[Azure Quantum]] service (batched)** - submit multiple param configurations as a single job to get the full Pareto frontier in one call.

| Field | Description |
|---|---|
| `physicalQubits` | Total physical [[Qubits]] needed (algorithm + T-factories) |
| `logicalQubits` | Logical [[Qubits]] used by the algorithm |
| `logicalDepth` | Number of logical cycles |
| `codeDistance` | Surface/floquet code distance $d$ (determines error suppression) |
| `tStates` | Total T-states consumed by the algorithm |
| `tFactories` | Number of T-factory copies running in parallel |
| `tFactoryFraction` | Fraction of total [[Qubits]] used by T-factories |
| `runtime` | Estimated wall-clock runtime |
| `rQOPS` | Reliable quantum operations per second of the target |
| `errorBudget` | Allowed failure probability (split across logical errors, T-distillation, rotations) |

## T-Factories

T-gates are non-Clifford & cannot be directly fault-tolerantly executed - they must be *distilled* from noisy T-states via magic state distillation. The estimator models T-factories as separate qubit blocks running in parallel:
- More T-factory copies → more physical [[Qubits]], but shorter runtime (T-states produced faster)
- Fewer T-factory copies → fewer [[Qubits]], but longer runtime (algorithm idles waiting for T-states)

This is the core **space-time tradeoff**: the Pareto frontier of (physicalQubits, runtime) pairs is plotted as a monotonically decreasing curve.

## QEC Schemes & Qubit Models

Two built-in QEC schemes:
- **[[Surface Code]]** - 2D lattice, each logical qubit requires $2d^2$ physical [[Qubits]], cycle time ∝ $d$
- **Floquet code** - honeycomb-based, more efficient use of connectivity, lower overhead in some regimes

Configurable qubit parameters (set `qubitParams` in the estimator job):
- `qubit_gate_ns_e3` - gate time $≈1 ns$, error rate ≈$10^{-3}$ (superconducting [[Qubits]])
- `qubit_gate_us_e3` - gate time $≈1 µs$, error rate ≈$10^{-3}$ (trapped ion [[Qubits]])
- Custom: specify `tGateTime`, `tGateErrorRate`, `twoQubitGateTime`, etc. directly

## Sources
- [Introduction to the Resource Estimator](https://learn.microsoft.com/en-us/azure/quantum/intro-to-resource-estimation)
- [Resource Estimator target parameters](https://learn.microsoft.com/en-us/azure/quantum/overview-resources-estimator)
- [Quickstart: use the Resource Estimator](https://learn.microsoft.com/en-us/azure/quantum/quickstart-microsoft-resources-estimator)
- [Space-time tradeoffs blog](https://quantum.microsoft.com/en-us/insights/blogs/resource-estimation/exploring-space-time-tradeoffs-with-azure-quantum-resource-estimator)
- [arXiv paper: using QRE for FTQC assessment](https://arxiv.org/abs/2311.05801)

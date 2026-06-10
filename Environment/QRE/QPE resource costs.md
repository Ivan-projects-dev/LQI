#Algorithm #SoftDev #Hardware
**[[QPE]] resource costs** - how to read quantum resource estimates for [[QPE]]-based algorithms and what benchmarks look like for real problems.

### Key resource quantities
See [[QRE]] for the Microsoft Azure estimator tool. The core quantities for any [[QPE]] circuit:

| Quantity | Definition | What drives it |
|----------|-----------|----------------|
| Logical qubits | Clock register ($t$) + system ($N$) + ancilla | Precision bits + Hamiltonian size |
| T-gate count | Dominant cost on [[Surface Code]] | $U^{2^k}$ applications, Toffoli gates |
| Code distance $d$ | Sets logical error rate $\sim (p/p_\text{th})^{(d+1)/2}$ | Target error budget |
| Physical qubits | $\sim 2d^2 \times$ logical qubits + T-factories | Code distance + T-factory count |
| Runtime | Logical depth × code cycle time × $d$ | Circuit depth + hardware speed |
| T-factory fraction | Physical qubits for T-distillation ÷ total | T-gate rate vs. compute rate |

### Why T-gates dominate
On fault-tolerant hardware ([[FTQC]]), Clifford gates (H, CNOT, S, CZ) are cheap - they can be done transversally on [[Surface Code]]. T-gates require **magic state distillation** (T-factories), which consumes most physical qubits and time.

**Rule of thumb**: 1 logical T-gate → $\sim$1000 physical qubits × 1 µs (superconducting). Reducing T-count by $10\times$ ≈ $10\times$ runtime reduction.

### Real benchmark: FeMoco (nitrogen fixation catalyst)
FeMoco (iron-molybdenum cofactor) is the industry benchmark for quantum chemistry QPE:

| Method | Logical qubits | Logical T-gates | Notes |
|--------|---------------|----------------|-------|
| Trotter (2017) | 111 | $\sim 10^{15}$ | First fault-tolerant estimate |
| [[Qubitization]] (2020) | 1,137 | $3.4\times 10^8$ | Babbush et al., factor $10^6\times$ improvement |
| [[Tensor]] hypercontraction + [[Qubitization]] (2021) | 2,196 | $6.7\times 10^9$ [[Toffoli]] | Different active space |
| Early [[FTQC]] (2025) | $< 10^4$ physical | Short circuits | Chemically accurate H₂ demonstrated |

Physical qubit count ([[Surface Code]], $d\sim 17$, $p=10^{-3}$): $\sim$4 million physical [[Qubits]] for full FeMoco → beyond 2025 hardware (~1,000 physical [[Qubits]]).

### Scaling formulas

**Total T-gates** for [[QPE]] via [[Trotterization]] (1st order):
$$N_T = O\!\left(\frac{m N^4 \Lambda^2 t^2}{\epsilon}\right) \times t_{\text{precision bits}}$$

**Total T-gates** for [[QPE]] via [[Qubitization]]:
$$N_T = O\!\left(\frac{\lambda t}{\epsilon}\right) \times O(\log N) \text{ per query}$$
where $\lambda = \sum_k |\alpha_k|$ (1-norm), $t$ = evolution time, $\epsilon$ = energy accuracy.

**Physical [[Qubits]]**:
$$Q_{\text{phys}} \approx 2d^2 \times Q_{\text{logical}} + Q_{\text{T-factories}}$$
T-factory fraction is typically 50–90% of total physical [[Qubits]].

### Precision ↔ clock qubits
For energy accuracy $\epsilon$ (in hartree) with evolution time $\tau$:
$$t_{\text{clock}} = \left\lceil\log_2\frac{1}{\epsilon\tau/(2\pi)}\right\rceil + O(\log(1/\delta))$$
Typical: $\epsilon = 10^{-3}$ hartree, $\tau = 10$ → $t \approx 14$ clock [[Qubits]].

### Microsoft Azure QRE tool
```python
# In Azure Quantum / Q# notebook
from azure.quantum import Workspace
from azure.quantum.target import MicrosoftEstimator

# Submit resource estimation job
job = estimator.submit(
    qpe_program,
    input_params={
        "errorBudget": 0.001,
        "qubitParams": {"name": "qubit_gate_ns_e3"},
        "qecScheme": {"name": "surface_code"}
    }
)
result = job.get_results()
print(result["physicalQubits"], result["tStates"], result["runtime"])
```

See [[QRE]] for full field descriptions and configuration options.

### Early fault-tolerant horizon (2025–2027)
Chemically accurate [[QPE]] for H₂ (4 spin-orbitals): demonstrated with $< 10^4$ physical [[Qubits]] using [[Iterative QPE]] + randomized Trotter + [[Error Mitigation]].

Next milestone: small molecules (6–10 spin-orbitals) at $< 10^5$ physical [[Qubits]]. Utility-scale chemistry (FeMoco): requires $\sim 4\times 10^6$ physical [[Qubits]] - estimated 2030s.

Sources: [Babbush et al. 2020](https://arxiv.org/abs/1902.02134), [Lee et al. 2021 (tensor hypercontraction)](https://arxiv.org/abs/2011.03531), [Webber et al. 2021 hardware impact](https://www.sussex.ac.uk/physics/iqt/wp-content/uploads/2021/11/Webber-2021.pdf), [Early FTQC QPE assessment 2024](https://arxiv.org/abs/2403.00077)

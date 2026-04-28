#SoftDev #Python
**D-Wave Leap** (`cloud.dwavesys.com`) is D-Wave's real-time quantum cloud service providing access to **[[Quantum annealing]]** processors. Unlike gate-model platforms, D-Wave uses **adiabatic quantum optimization** - best suited for combinatorial optimization, sampling, & constraint satisfaction problems, not general quantum circuits.
- **Leap Quantum LaunchPad** (announced Jan $2025$) - $3$-month free program for qualified startups, researchers, & enterprises. Includes:
  - Full QPU access (Advantage2 system)
  - Access to **hybrid solvers** (classical + quantum combination)
  - Expert support from D-Wave quantum engineers
- Standard Leap accounts get limited free time allocation for exploration

**Advantage2** (GA May $2025$):
- **$4400+$ [[Qubits]]** in Pegasus topology
- **Fast anneal** feature - rapid computation cycles with improved noise resistance
- Sub-second solve times for optimization problems
- $99.9$% uptime SLA

Previous generation: **Advantage** ($5000+$ [[Qubits]], Pegasus graph). D-Wave [[Qubits]] are **flux [[Qubits]]** - physically different from gate-model transmon or trapped-ion [[Qubits]]. They are not universal quantum computers; they solve [[QUBO]]/Ising problems natively.

SDK: **Ocean** (Python). Problems are formulated as **[[QUBO]] (Quadratic Unconstrained Binary Optimization)** or **[[Ising model]]** & submitted to the annealer.

```python
import dimod
from dwave.system import DWaveSampler, EmbeddingComposite

# Define a simple Ising problem: minimize -s0*s1
sampler = EmbeddingComposite(DWaveSampler())
response = sampler.sample_ising({'s0': 0.0, 's1': 0.0}, {('s0','s1'): -1}, num_reads=100)
print(response.first.sample) # {'s0': 1, 's1': 1} or {-1, -1}
```

**Hybrid solvers** (Leap Hybrid) combine QPU + classical compute for larger problems (thousands to millions of variables):
- `LeapHybridSampler` - general [[QUBO]]/BQM
- `LeapHybridCQMSampler` - constrained quadratic models (handles inequalities)

Use cases - combinatorial optimization: vehicle routing, job scheduling, portfolio optimization, network design, drug discovery (molecular docking). Not suitable for [[Grover]] search, [[QPE]], [[Shor]]'s algorithm - those require gate-model hardware.

**[[QUBO]] formulation is the entire skill.** The annealer minimizes $$\sum_{i} h_i s_i + \sum_{i<j} J_{ij} s_i s_j$$Translating real problem (TSP, scheduling, portfolio) into this quadratic form is the hard part. Study the [Ocean Problem Formulation guide](https://docs.ocean.dwavesys.com) before writing any code.

**Embedding is automatic but costly.** `EmbeddingComposite` maps logical variables onto physical qubits via chains. $10$-variable problem might use $50$ physical qubits due to chains. Check `response.info["timing"]` & `response.info["embedding_context"]` to see what the mapper did.

**Chain strength tuning.** Chain strength controls how strongly chained qubits are forced to agree. Default is usually fine; if you see many broken chains (`chain_break_fraction > 0.1` in results), increase chain strength. Too high $→$ suppresses the quantum effect; too low $→$ chain breaks dominate.

**Hybrid solvers are almost always the right choice for real apps.** Raw QPU access is for research & benchmarking. `LeapHybridSampler` handles constraint satisfaction, inequality constraints, & variables in the thousands. It outperforms raw QPU on all but the smallest, densest graphs.

**D-Wave gives a distribution, not a single answer.** `num_reads=1000` returns $1000$ candidate solutions, each with energy value. Take `response.first.sample` for the best, or look at the full distribution to understand the energy landscape.

D-Wave is best for discrete optimization; IBM/Azure/Braket are for [[Quantum simulation]] & algorithm research.
## Sources
- [D-Wave Leap cloud service](https://cloud.dwavesys.com)
- [Leap Quantum LaunchPad announcement](https://www.dwavequantum.com/company/newsroom/press-release/d-wave-announces-new-leap-quantum-launchpad-program-to-fast-track-deployment-of-quantum-computing-applications/)
- [Advantage2 GA announcement](https://thequantuminsider.com/2025/05/20/d-wave-announces-general-availability-of-advantage2-quantum-computer/)
- [Ocean SDK documentation](https://docs.ocean.dwavesys.com)
- [Ocean SDK GitHub](https://github.com/dwavesystems/dwave-ocean-sdk)

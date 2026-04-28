#SoftDev #Hardware 
[[D-Wave]] is **not** a gate-based quantum computer. There are no Hadamard gates, no CNOTs, no circuit diagrams. It does not run any of the algorithms in standard quantum computing textbooks ([[Grover]], [[Shor]], [[QPE]], [[VQE]]).

[[D-Wave]] is a **quantum annealer** - machine built specifically to find min-energy configurations of optimization problems. It is best understood as very fast, physics-based heuristic solver for combinatorial optimization. If your goal is learning quantum gates & circuits, use [[IBM Quantum]] first. If your goal is solving real optimization problems (routing, scheduling, finance), [[D-Wave]] is the right tool.
[[D-Wave]] solves problems expressed as energy functions. You define math expression over binary variables where lower energy $=$ better solution. Annealer finds the configuration of variables with the lowest energy.

**[[QUBO]]form:** $$E = \sum_i Q_{ii} x_i + \sum_{i < j} Q_{ij} x_i x_j$$ where $x_i \in \{0, 1\}$ are binary variables & $Q$ is [[Matrix]] of coefficients you define.
```bash
pip install dwave-ocean-sdk
dwave config create # prompts for API token from cloud.dwavesys.com
```
Create free account at `cloud.dwavesys.com`. You get small free-time allocation for exploration.
## Sources
- [D-Wave Ocean SDK getting started](https://docs.ocean.dwavesys.com/en/stable/getting_started.html)
- [QUBO problem formulation](https://docs.ocean.dwavesys.com/en/stable/concepts/samplers.html)
- [Leap Hybrid Solvers](https://docs.ocean.dwavesys.com/en/stable/docs_hybrid/reference/samplers.html)
- [D-Wave problem formulation guide](https://docs.dwavesys.com/docs/latest/c_gs_workflow.html)
- [Max-Cut tutorial](https://docs.dwavesys.com/docs/latest/handbook_maxcut.html)

#Quantum #Beginner #Annealing
Concrete walkthrough of starting with [[D-Wave Leap]] - which works on completely different principles from IBM, Azure, and Braket.

## The Critical Difference

D-Wave is **not** a gate-based quantum computer. There are no Hadamard gates, no CNOTs, no circuit diagrams. It does not run any of the algorithms in standard quantum computing textbooks ([[Grover]], Shor, [[QPE]], [[VQE]]).

D-Wave is a **quantum annealer** - a machine built specifically to find minimum-energy configurations of optimization problems. It is best understood as a very fast, physics-based heuristic solver for combinatorial optimization.

If your goal is learning quantum gates and circuits, use [[IBM Quantum]] first. If your goal is solving real optimization problems (routing, scheduling, finance), D-Wave is the right tool.

## The Core Concept: Energy Minimization

D-Wave solves problems expressed as energy functions. You define a mathematical expression over binary variables where lower energy = better solution. The annealer finds the configuration of variables with the lowest energy.

**The [[QUBO]] (Quadratic Unconstrained Binary Optimization) form:**
$E = \sum_i Q_{ii} x_i + \sum_{i < j} Q_{ij} x_i x_j$

where $x_i \in \{0, 1\}$ are binary variables and $Q$ is a [[Matrix]] of coefficients you define.

**Analogy:** imagine a hilly landscape where every point represents a possible assignment of your variables, and the height is the cost of that assignment. D-Wave uses quantum tunneling to hop through barriers and find deep valleys (low-cost solutions) that classical hill-climbing would miss.

## Installation and Authentication

```bash
pip install dwave-ocean-sdk
dwave config create    # prompts for API token from cloud.dwavesys.com
```

Create a free account at `cloud.dwavesys.com`. You get a small free-time allocation for exploration.

## Your First Problem

```python
from dwave.system import DWaveSampler, EmbeddingComposite

# Problem: $2$ binary variables x0, x1
# Minimize: -x0*x1 (lowest energy when both are 1)
sampler = EmbeddingComposite(DWaveSampler())
response = sampler.sample_ising(
    {},                  # linear biases (none here)
    {(0, 1): -1},        # quadratic: coupling between x0 and x1
    num_reads=100
)
print(response.first.sample)   # {0: 1, 1: 1}
print(response.first.energy)   # -1.0
```

`num_reads=100` means the annealer runs $100$ times, giving you $100$ candidate solutions. `response.first` is the lowest-energy one.

## A Real Optimization Problem: Max-Cut

Find a partition of graph nodes that maximizes edges crossing the partition boundary:

```python
import dimod
from dwave.system import LeapHybridSampler

# Graph: nodes 0,1,2,3 with edges
edges = [(0,1), (1,2), (2,3), (3,0), (0,2)]
Q = {}
for u, v in edges:
    Q[(u, u)] = Q.get((u, u), 0) - 1
    Q[(v, v)] = Q.get((v, v), 0) - 1
    Q[(u, v)] = Q.get((u, v), 0) + 2

sampler = LeapHybridSampler()   # hybrid classical+quantum solver
result = sampler.sample_qubo(Q, time_limit=5)
print(result.first.sample)      # dict of node: 0 or 1 (which partition)
```

`LeapHybridSampler` combines QPU + classical heuristics. For real problems, it almost always outperforms raw QPU access and handles much larger variable counts.

## What D-Wave Cannot Do

- Run [[Grover]]'s algorithm
- Simulate quantum chemistry
- Execute any gate-model algorithm
- Replace IBM/Azure for algorithm research

D-Wave and gate-model platforms solve different problem classes. They are not competing - they are complementary.

## Sources
- [D-Wave Ocean SDK getting started](https://docs.ocean.dwavesys.com/en/stable/getting_started.html)
- [QUBO problem formulation](https://docs.ocean.dwavesys.com/en/stable/concepts/samplers.html)
- [Leap Hybrid Solvers](https://docs.ocean.dwavesys.com/en/stable/docs_hybrid/reference/samplers.html)
- [D-Wave problem formulation guide](https://docs.dwavesys.com/docs/latest/c_gs_workflow.html)
- [Max-Cut tutorial](https://docs.dwavesys.com/docs/latest/handbook_maxcut.html)

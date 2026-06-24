#SoftDev #Q-Sharp #Algorithm Max-Cut in [[D-Wave]]

Find partition of graph nodes that maximizes edges crossing the partition boundary:
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

sampler = LeapHybridSampler() # hybrid classical+quantum solver
result = sampler.sample_qubo(Q, time_limit=5)
print(result.first.sample) # dict of node: 0 or 1 (which partition)
```
`LeapHybridSampler` combines QPU + classical heuristics. For real problems, it almost always outperforms raw QPU access & handles much larger variable counts.

Source: [D-Wave Ocean SDK documentation](https://docs.ocean.dwavesys.com/en/stable/) [dwave-examples — GitHub](https://github.com/dwave-examples)


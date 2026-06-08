#SoftDev #Hardware #Math #Experience
`pip install dwave-ocean-sdk`, `dwave config create` with your API token. 

`SimulatedAnnealing` runs locally free with no cloud calls. Submitting [[QUBO]] to real hardware is $1$ func call: `sampler.sample_qubo(Q, num_reads=100)`. If you already have your problem in [[QUBO]] form, the code is $\sim 5$ lines.


**There are no gates, no circuits, no [[Qubits]] in the traditional sense.** [[D-Wave]] is quantum annealer. It does not run [[Grover]], [[Shor]], [[VQE]], or [[QPE]]. It cannot. Entire paradigm is different: you define energy func over binary variables, & the machine finds the min-energy configuration. Comparing [[D-Wave]] to IBM is like comparing graphics card to CPU - different tools for different jobs.

**[[QUBO]] formulation IS the programming.** code to submit problem is trivial. Hard part is expressing your problem as:
$$E = \sum_i Q_{ii} x_i + \sum_{i<j} Q_{ij} x_i x_j$$
where lower energy $=$ better solution. There is no way to debug this with print statements - you must verify the formulation by enumerating all $2^n$ states on a tiny $2$–$3$ variable instance & checking that the correct answer has the lowest energy.

**Chain breaks give physically invalid results, not just suboptimal ones.** When `chain_break_fraction > 0.05`, the physical [[Qubits]] representing a logical variable disagree. The returned solution is garbage regardless of its energy value. Check this before trusting any result:
```python
print(sampleset.first.chain_break_fraction)  # must be < 0.05
```

**`EmbeddingComposite` recomputes embedding on every call.** Embedding your logical problem onto the physical Pegasus graph takes seconds. If you call `sample_qubo` in a loop with the same problem structure, it recomputes the embedding each time. Cache it:
```python
from dwave.system import DWaveSampler, FixedEmbeddingComposite
from minorminer import find_embedding

sampler = DWaveSampler()
embedding = find_embedding(list(Q.keys()), sampler.edgelist)
cached = FixedEmbeddingComposite(sampler, embedding)
```

**Results are heuristic.** Running the same [[QUBO]] $1000$ times can give different answers. [[D-Wave]] is not guaranteed to find the global min - it is a physical heuristic. More reads increase the probability of finding the best solution but don't guarantee it. The energy histogram tells you how reliably the annealer is finding the min - a tight cluster at one energy value is good, a wide spread means it's getting stuck.

**[[QUBO]] formulation for non-trivial problems is hard.** Penalty terms for constraints (equality constraints, inequality constraints, multi-variable interactions) must be carefully weighted. 
Penalty too weak lets infeasible solutions win. Penalty too strong squashes the signal. 
Getting the balance right is the central skill & the main reason [[D-Wave]] problems take time to solve.

**Dense problem graphs produce long chains.** Fully connected problems ($K_n$ graphs) require each logical variable to be represented as chain of multiple physical [[Qubits]]. Chains longer than $5$ are fragile. For dense problems, use `LeapHybridSampler` which handles them natively without manual embedding.

**Free tier time is limited.** [[D-Wave]]'s free Leap allocation covers small experiments. Serious optimization problems with hundreds of variables require credits or academic access.

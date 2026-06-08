#SoftDev #Python 
[[D-Wave]] has no circuit to debug - you debug the **[[QUBO]] formulation** & the **embedding**. Most wrong results come from one of $3$ sources: chain breaks, wrong [[QUBO]] coefficients, or annealer getting stuck in local minima.
### Chain Breaks
Physical [[Qubits]] representing the same logical variable disagree - the result is physically invalid, not just suboptimal. Chain break fraction $> 0.05$ means results are garbage.
```python
sampleset = sampler.sample_qubo(Q, num_reads=100, chain_strength=2.0)
print("Chain break fraction:", sampleset.first.chain_break_fraction)
```
Fix: increase `chain_strength`. Starting value - max absolute coefficient in your [[QUBO]]:
```python
max_coeff = max(abs(v) for v in Q.values())
chain_strength = max_coeff * 1.5
```
### Wrong QUBO Coefficients
Chain breaks $= 0$ but answers are still wrong means the formulation is wrong. Verify on a tiny instance you can solve by hand: $2$-$3$ variables, enumerate all $2^n$ states manually & check which has the lowest energy under your $Q$.
```python
import itertools

variables = [0, 1, 2]
for bits in itertools.product([0, 1], repeat=len(variables)):
    x = dict(zip(variables, bits))
    energy = sum(Q.get((i, j), 0) * x[i] * x[j] for i in variables for j in variables)
    print(bits, "->", energy)
```
The configuration with the lowest energy should match your expected solution. If it does not, the bug is in how you built $Q$.
### Energy Histogram - Stuck in Local min
```python
import matplotlib.pyplot as plt

energies = sampleset.record.energy
plt.hist(energies, bins=20)
plt.xlabel("Energy")
plt.show()
```
If the histogram shows a tight cluster at one energy value: annealer consistently found the same solution - likely the global min. If it shows a wide spread: annealer is getting stuck in different local minima. Fix: increase `num_reads` or switch to `LeapHybridSampler`.
### Embedding
```python
from dwave.system import DWaveSampler
from minorminer import find_embedding

sampler = DWaveSampler()
embedding = find_embedding(list(Q.keys()), sampler.edgelist)
print("Longest chain:", max(len(v) for v in embedding.values()))
```
Chains longer than $5$ physical [[Qubits]] break frequently. If your problem produces long chains, reduce variable connectivity in the [[QUBO]] or use `LeapHybridSampler` which handles dense problems without manual embedding.

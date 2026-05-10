#SoftDev #Python 
### Checking Chain Breaks
```python
sampleset = sampler.sample_qubo(Q, num_reads=100, chain_strength=2.0)

# fraction of reads with broken chains
print("Chain break fraction:", sampleset.first.chain_break_fraction)
```
Chain break fraction $> 0.05$ ($5\%$) means your chain strength is too low. Physical [[Qubits]] representing logical variable disagree - the result is garbage.

Fix: increase `chain_strength`. Rough starting value is the max absolute coefficient in your [[QUBO]] [[Matrix]].
```python
max_coeff = max(abs(v) for v in Q.values())
chain_strength = max_coeff * 1.5
```
### Visualizing the Embedding
```python
from dwave.system import DWaveSampler, EmbeddingComposite
from minorminer import find_embedding

sampler = DWaveSampler()
source_edgelist = list(Q.keys())
target_edgelist = sampler.edgelist
embedding = find_embedding(source_edgelist, target_edgelist)
print("Longest chain:", max(len(v) for v in embedding.values()))
print("num of chains:", len(embedding))
```
Long chains (length $> 5$) are fragile under noise. Reformulate your problem to reduce connectivity, or use `LeapHybridSampler` which handles dense problems natively.

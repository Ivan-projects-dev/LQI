#SoftDev 
###  `SolverNotFoundError` or auth failure
Config wasn't created or has a wrong token. Re-run the setup & verify:
```bash
dwave config create # interactive: enter API token from cloud.dwavesys.com
dwave ping # should print: "PING ... QPU online"
```
###  Solution quality is poor / random-looking. 
Usually chain strength problem. Check `chain_break_fraction` in the response:
```python
print(response.first.chain_break_fraction)   # should be < 0.05
```
If it's high (> 0.1), increase chain strength:
```python
response = sampler.sample_ising(h, J, num_reads=100, chain_strength=2.0)
# default is usually 1.0 - try 1.5, 2.0, 3.0
```
###  Embedding takes minutes
`EmbeddingComposite` computes the embedding fresh each call. Cache it for repeated runs on the same problem structure:
```python
from dwave.system import DWaveSampler, FixedEmbeddingComposite
from minorminer import find_embedding

sampler = DWaveSampler()
embedding = find_embedding(Q, sampler.edgelist)
cached = FixedEmbeddingComposite(sampler, embedding)
# now cached.sample_qubo(Q) reuses the same embedding every time
```
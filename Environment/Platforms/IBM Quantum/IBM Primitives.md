#SoftDev #Python #Hardware 
[[IBM Quantum]] hardware is accessed through $2$ primitives: `SamplerV2` for measurement distributions & `EstimatorV2` for expectation values. They replaced `backend.run()` & `execute()` in Qiskit $1.0$. Choosing the wrong one wastes QPU time & produces confusing output.

**`SamplerV2`** - run circuit & collect measurement outcomes as bit-string counts. Use for: any circuit where you care about the distribution of results (Bell state, Grover, QAOA landscape). 

Output: `PubResult.data.<register_name>` as a `BitArray`.

**`EstimatorV2`** - compute expectation value $\langle\psi|O|\psi\rangle$ without measuring the full state. Use for: [[VQE]], [[QAOA]] cost func evaluation, any variational algorithm. Output: `PubResult.data.evs` as numpy array of floats.
### PUBs - The Input Format
Both primitives take a list of **PUBs (Primitive Unified Blocs)**. Each PUB is tuple:
Sampler PUB: (`circuit, parameter_values, shots`)
Estimator PUB: (`circuit, observables, parameter_values, precision`)

Multiple PUBs $=$ multiple circuits in single job. Results come back as list of `PubResult` objects, $1$ per PUB.
```python
from qiskit_ibm_runtime import SamplerV2
from qiskit import transpile, QuantumCircuit

qc = QuantumCircuit(2)
qc.h(0); qc.cx(0, 1); qc.measure_all()
tqc = transpile(qc, backend, optimization_level=3)
sampler = SamplerV2(backend)
job = sampler.run([tqc], shots=1024)
result = job.result()
# Extract counts - register name matches how you added measurements
counts = result[0].data.meas.get_counts() # measure_all() creates 'meas'
# counts = result[0].data.c.get_counts() # qc.measure([0,1],[0,1]) creates 'c'
```
**Register name is the `ClassicalRegister` name, not always `meas`.** If you get `AttributeError: 'DataBin' object has no attribute 'meas'`, check the register name:
```python
reg_name = list(result[0].data)[0]
counts = getattr(result[0].data, reg_name).get_counts()
```

```python
from qiskit_ibm_runtime import EstimatorV2
from qiskit.quantum_info import SparsePauliOp

observable = SparsePauliOp("ZZ")  # measure ZZ correlation
estimator = EstimatorV2(backend)
job = estimator.run([(tqc, [observable])])
result = job.result()
evs = result[0].data.evs # expectation values array
stds = result[0].data.stds # standard deviations
print(evs)  # e.g. 0.964 for near-perfect Bell state
```
### Local Simulation Without Hardware
Use `StatevectorSampler` / `StatevectorEstimator` for noiseless local simulation - no backend, no credentials:
```python
from qiskit.primitives import StatevectorSampler, StatevectorEstimator

sampler = StatevectorSampler()
job = sampler.run([tqc])
counts = job.result()[0].data.meas.get_counts()
```
`AerSimulator` + `FakeBackend` for noise-realistic local simulation - see [[IBM Debug]].

| Goal | Primitive |
|---|---|
| Check measurement distribution (does circuit give right bit strings?) | `SamplerV2` |
| Compute energy / cost function / correlation | `EstimatorV2` |
| Train variational circuit ([[VQE]], [[QAOA]]) | `EstimatorV2` |
| Count & histogram outcomes | `SamplerV2` |

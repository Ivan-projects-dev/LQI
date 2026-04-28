#SoftDev #Python 
###  `AttributeError: 'QiskitRuntimeService' object has no attribute 'run'` 
You're using old workflow. `QiskitRuntimeService` doesn't run circuits directly. Use **primitive** (`SamplerV2` or `EstimatorV2`):
```python
# WRONG (old API, removed in Qiskit 1.0)
job = service.run(qc, backend=backend)
# CORRECT
sampler = SamplerV2(backend)
job = sampler.run([transpile(qc, backend)], shots=1024)
```
###  `IBMInputValueError: 'The instruction ... is not supported'`
Your circuit contains gate the backend doesn't support natively. You forgot to transpile:
```python
# Always transpile before submitting
from qiskit import transpile
tqc = transpile(qc, backend, optimization_level=3)
sampler.run([tqc]) # submit the transpiled circuit, not the original
```
###  `KeyError: 'meas'` when extracting counts
Measurement register name doesn't match. `measure_all()` creates register named `meas`. If you used `measure(0, 0)` manually, the register name is whatever you named it (default `c`):
```python
# With measure_all():
counts = result[0].data.meas.get_counts()
# With QuantumCircuit(2, 2) + qc.measure([0,1], [0,1]):
counts = result[0].data.c.get_counts() # register named 'c'
# Safe universal way:
pub = result[0]
reg_name = list(pub.data)[0] # get whatever the register is called
counts = getattr(pub.data, reg_name).get_counts()
```
###  Circuit runs but all results are `00000`
You forgot `measure_all()` or any measurement instruction. Circuit submitted without measurements returns all $0$s. Add `qc.measure_all()` before transpiling.
###  `QiskitRuntimeError: No IBM Quantum account found`
API token isn't saved yet. Run this **once** to store it permanently:
```python
from qiskit_ibm_runtime import QiskitRuntimeService
QiskitRuntimeService.save_account(
    channel="ibm_quantum",
    token="YOUR_TOKEN_HERE", # from quantum.cloud.ibm.com → Account settings
    overwrite=True)
```
After saving, `QiskitRuntimeService()` works without arguments.
###  `qc.draw('mpl')` crashes with `ModuleNotFoundError`
Matplotlib circuit diagrams need two extra packages:
```bash
pip install matplotlib pylatexenc
```
###  Transpiled circuit is much deeper than expected
`optimization_level=0` does minimal rewriting. Always use level 3 for real hardware:
```python
tqc = transpile(qc, backend, optimization_level=3)
print(f"Original depth: {qc.depth()}, Transpiled: {tqc.depth()}")
```
$5$-gate circuit at level $0$ may become $30$ gates; at level $3$ it may stay at $8$. Difference matters enormously on noisy hardware.
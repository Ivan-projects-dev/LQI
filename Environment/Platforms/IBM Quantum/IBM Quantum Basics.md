#SoftDev #Python 
Visit `quantum.cloud.ibm.com`. Create IBM ID - no credit card required. The dashboard shows real quantum computers you can access: their qubit counts, current queue lengths, & live calibration data (gate error rates, $T_1/T_2$ coherence times). "Least busy" backend is usually the safest choice. Queue lengths are shown in minutes - during peak hours they can be $1$–$3$ hours.

Python code using the **Qiskit** library. Circuit is `QuantumCircuit` object. [[Qubits]] are indexed from $0$.
```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(2) # 2 qubits, both start as |0⟩
qc.h(0) # H gate: put qubit 0 in 50/50 superposition
qc.cx(0, 1) # CNOT: entangle qubit 0 (control) & qubit 1 (target)
qc.measure_all() # add measurement to all qubits
qc.draw() # prints ASCII circuit diagram
```
This is **Bell state** - simplest entanglement demonstration. Theory: you only ever get `00` or `11`, never `01` or `10`. The $2$ [[Qubits]] are perfectly correlated. But always test on simulator before spending QPU time:
```python
from qiskit_aer import AerSimulator

sim = AerSimulator()
job = sim.run(qc, shots=1024)
result = job.result()
counts = result.get_counts()
print(counts)  # {'00': 512, '11': 512} - perfect, no noise
```
`shots=1024` means the circuit runs $1024$ times. You get a dictionary of bit strings & how often each appeared. This is the **[[Probability distribution]]** the quantum circuit produces.
### Running on Real Hardware
```python
from qiskit_ibm_runtime import QiskitRuntimeService, SamplerV2
from qiskit import transpile

service = QiskitRuntimeService() # requires saved API token
backend = service.least_busy(operational=True, simulator=False)
tqc = transpile(qc, backend, optimization_level=3)  # compile for native gate set
sampler = SamplerV2(backend)
job = sampler.run([tqc], shots=1024)
result = job.result()
```
**What changes on real hardware:**
- Results might be `{'00': 490, '11': 507, '01': 15, '10': 12}` instead of perfect `512/512`
- $27$ wrong answers are physical errors - not bugs
- `transpile()` rewrites your circuit for the chip's native gate set (`ECR`, `RZ`, `SX`, `X`)

`SamplerV2` returns `PrimitiveResult`. Extract counts:
```python
pub_result = result[0] # first circuit's result
counts = pub_result.data.meas.get_counts() # measurement register named "meas"
```
Counts dictionary tells you how often each classical bit string appeared. $>$ shots = smoother distribution.
###  Sources
- [IBM Quantum Learning - Basics of Quantum Information](https://learning.quantum.ibm.com/course/basics-of-quantum-information)
- [Qiskit Getting Started](https://docs.quantum.ibm.com/start)
- [SamplerV2 documentation](https://docs.quantum.ibm.com/api/qiskit-ibm-runtime/sampler-v2)
- [Hello World tutorial](https://docs.quantum.ibm.com/guides/hello-world)

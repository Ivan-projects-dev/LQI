#SoftDev **Batch Your Jobs**

Every QPU job has overhead: queue wait time, calibration, readout. Submitting $10$ circuits in one batch takes roughly the same queue time as $1$ circuit. 

**[[IBM Quantum]] - submit a list of circuits:**
```python
from qiskit_ibm_runtime import SamplerV2
from qiskit import transpile

circuits_to_run = [tqc_1, tqc_2, tqc_3, tqc_4, tqc_5]
sampler = SamplerV2(backend)
job = sampler.run(circuits_to_run, shots=1024)
result = job.result()

for i, pub_result in enumerate(result):
    counts = pub_result.data.meas.get_counts()
    print(f"Circuit {i}: {counts}")
```
Single job with $5$ circuits uses $5x$ info for the same wait time.

**[[Amazon Braket]] - batch tasks:**
```python
from braket.aws import AwsQuantumTaskBatch

tasks = device.run_batch(circuits, s3_destination_folder=s3_folder, shots=1000)
results = tasks.results()  # waits for all to complete
```

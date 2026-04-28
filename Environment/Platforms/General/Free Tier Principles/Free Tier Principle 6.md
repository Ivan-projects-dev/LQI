#SoftDev #Python **Save Job IDs Immediately**

QPU jobs are asynchronous. You submit, wait, come back later. If your Python session dies, the job is still running.
```python
job = sampler.run([tqc], shots=1024)
job_id = job.job_id()
print("SAVE THIS:", job_id)

# Later, in a new session:
from qiskit_ibm_runtime import QiskitRuntimeService
service = QiskitRuntimeService()
job = service.job(job_id)
result = job.result()
```
Keep text file of job IDs with descriptions. Do not rely on memory or session state.

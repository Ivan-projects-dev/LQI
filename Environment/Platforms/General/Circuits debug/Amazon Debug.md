#Python #SoftDev 
### LocalSimulator for Free, Instant Testing
```python
from braket.devices import LocalSimulator

device = LocalSimulator()
task = device.run(circuit, shots=1000)
result = task.result()
print(result.measurement_counts)
```
`LocalSimulator` runs in your process with no API calls, no S3 buckets, no queue. It supports up to $~25$ [[Qubits]] before it becomes slow. Always debug here.
### Inspecting Circuit Structure
```python
print(circuit)
# prints ASCII diagram of the circuit

print(circuit.depth) # gate depth
print(circuit.qubit_count) # num of qubits
```
### Reading QPU Results
On real QPU job:
```python
task = device.run(circuit, s3_destination_folder=("my-bucket", "prefix"), shots=1000)
task_id = task.id
print("Task ID:", task_id)

# retrieve later
from braket.aws import AwsQuantumTask
task = AwsQuantumTask(arn=task_id)
result = task.result()   # blocks until complete
print(result.measurement_counts)
```

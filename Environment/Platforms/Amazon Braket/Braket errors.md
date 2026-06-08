#SoftDev #Python 
1. `ValueError: [[[[S3]]]] bucket ... does not exist`
You must create an [[S3]] bucket before submitting cloud or QPU tasks. The bucket name must be unique globally & exist in the same AWS region as your Braket workspace:
```python
task = device.run(circuit, shots=100, s3_destination_folder=("my-braket-results-bucket", "subfolder/")) # ^ bucket must exist in AWS S3
```
Create the bucket in the AWS console $→ [[[[S3]]]] →$ Create bucket $→$ choose your region.
2. Task shows as `COMPLETED` but `task.result()` hangs
Result is being downloaded from [[S3]]. If the task just finished, wait $5-10$ seconds & retry. For large results (many shots, many [[Qubits]]), the [[S3]] download takes time. Add short delay:
```python
import time
while task.state() != "COMPLETED":
    time.sleep(2)
result = task.result()
```
3. `ClientError: error occurred (ValidationException)`
Usually means the circuit contains gate the selected device doesn't support. Check the device's supported gate set:
```python
device = AwsDevice("arn:aws:braket:...")
print(device.properties.action['braket.ir.openqasm.program'].supportedOperations)
```

#Python #SoftDev 
`LocalSimulator` runs in your process - no API calls, no [[S3]], no queue, no cost. Always start here. If it fails, it is circuit logic. If it passes but QPU fails, it is noise or connectivity.
```python
from braket.devices import LocalSimulator

device = LocalSimulator()
result = device.run(circuit, shots=1000).result()
print(result.measurement_counts)
```
### Inspecting Circuit Structure
```python
print(circuit) # ASCII diagram
print(circuit.depth) # gate depth
print(circuit.qubit_count)
```
### Checking Unsupported Gates
Each QPU has a supported gate set. Submitting a circuit with an unsupported gate gives `ValidationException` with no useful gate name. Check what the device supports before submitting:
```python
device = AwsDevice("arn:aws:braket:us-east-1::device/qpu/ionq/Aria-1")
print(device.properties.action['braket.ir.openqasm.program'].supportedOperations)
# ['H', 'CNOT', 'Rx', 'Ry', 'Rz', ...]  - if your gate isn't here, decompose it first
```
### Distinguishing Logic Bug From Noise
Run identical circuit on `LocalSimulator` (noiseless) & `DM1` simulator (noisy) before touching QPU:
```python
from braket.aws import AwsDevice

dm1 = AwsDevice("arn:aws:braket:::device/quantum-simulator/amazon/dm1")
result = dm1.run(circuit, shots=1000).result()
print(result.measurement_counts)
```
If `LocalSimulator` gives correct distribution & `DM1` degrades it, noise is the issue - reduce circuit depth. If `LocalSimulator` already gives wrong distribution, fix the logic first.

Source: [Amazon Braket documentation](https://docs.aws.amazon.com/braket/)
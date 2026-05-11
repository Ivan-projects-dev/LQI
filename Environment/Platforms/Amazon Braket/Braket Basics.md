#Python #SoftDev #Hardware 
```python
from braket.devices import LocalSimulator
from braket.circuits import Circuit

device = LocalSimulator() # free, no AWS charges, no account needed for this
bell = Circuit().h(0).cnot(0, 1) # Bell state: H on qubit 0, CNOT(0→1)
task = device.run(bell, shots=1000)
result = task.result()
print(result.measurement_counts)   # {'00': 503, '11': 497}
```

Braket's `Circuit` uses method chaining: `.h(0)` adds an H gate, `.[[CNOT]](0, 1)` adds a [[CNOT]]. Identical to Qiskit in [[Logic]], slightly different syntax.

Your free tier gives $1$ hour/month of managed simulator time for the $1st$ $12$ months:
```python
from braket.aws import AwsDevice

# SV1: noiseless statevector simulator (up to 34 qubits)
device = AwsDevice("arn:aws:braket:::device/quantum-simulator/amazon/sv1")

task = device.run(bell, shots=1000, s3_destination_folder=("your-s3-bucket", "results/"))
result = task.result()
```
**Important:** cloud simulator results go to $S3$ first. You must create $S3$ bucket in the same region as your Braket workspace before running cloud tasks.

Same circuit (with minor adaptation) runs on completely different qubit technologies:
```python
# IonQ Aria - trapped-ion qubits
ionq = AwsDevice("arn:aws:braket:us-east-1::device/qpu/ionq/Aria-1")

# Rigetti Ankaa - superconducting qubits
rigetti = AwsDevice("arn:aws:braket:us-west-1::device/qpu/rigetti/Ankaa-3")
```

| | Superconducting (IBM/[[Rigetti]]) | Trapped ion ([[IonQ]]) |
|---|---|---|
| Gate speed | ~$50$ ns | ~$100$ μs |
| Coherence time | ~$100$ μs | ~seconds |
| All-to-all connectivity | No | Yes |
| Gate error per [[CNOT]] | ~$0.3$-$0.5$% | ~$0.3$% |
Running the same Bell state on both & comparing histograms teaches $>$ about quantum hardware than any textbook. The distributions will differ noticeably.

**Warning on QPU cost:** QPU jobs are paid - [[IonQ]] trapped-ion devices are among the more expensive options per shot. Test entirely on simulators first. Set AWS billing alerts.

**QuEra's Aquila** device is **not gate-based computer**. It uses neutral atoms & programs via laser pulses & atom positions - `AnalogHamiltonianSimulation` object, not `Circuit`. If you see Aquila on the device list, it requires completely different programming model.

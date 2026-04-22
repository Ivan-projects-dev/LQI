#Quantum #Beginner #AWS
Concrete walkthrough of starting with [[Amazon Braket]] without prior AWS experience.

## What Braket Is

Amazon's quantum computing service. Think of it as [[IBM Quantum]] from AWS, with $1$ key difference: Braket gives you access to **multiple hardware providers** through a single SDK - IonQ, Rigetti, QuEra, and IQM, all via the same Python interface.

Best used for: comparing hardware technologies, running circuits across providers, or integrating quantum work into an existing AWS workflow.

## First Steps

1. Create an AWS account at `aws.amazon.com`
2. Open Braket in the AWS console (search "[[Amazon Braket]]")
3. Install the SDK: `pip install amazon-braket-sdk`
4. **Start with the local simulator - it's always free and runs on your laptop**

## Your First Circuit (Local Simulator)

```python
from braket.devices import LocalSimulator
from braket.circuits import Circuit

device = LocalSimulator()   # free, no AWS charges, no account needed for this

bell = Circuit().h(0).cnot(0, 1)   # Bell state: H on qubit 0, CNOT(0→1)
task = device.run(bell, shots=1000)
result = task.result()

print(result.measurement_counts)   # {'00': 503, '11': 497}
```

Braket's `Circuit` uses method chaining: `.h(0)` adds an H gate, `.[[CNOT]](0, 1)` adds a [[CNOT]]. Identical to Qiskit in [[Logic]], slightly different syntax.

## Cloud Simulators (Free Tier)

Your free tier gives $1$ hour/month of managed simulator time for the first $12$ months:

```python
from braket.aws import AwsDevice

# SV1: noiseless statevector simulator (up to 34 qubits)
device = AwsDevice("arn:aws:braket:::device/quantum-simulator/amazon/sv1")

task = device.run(bell, shots=1000,
    s3_destination_folder=("your-s3-bucket", "results/"))
result = task.result()
```

**Important:** cloud simulator results go to S3 first. You must create an S3 bucket in the same region as your Braket workspace before running cloud tasks. This surprises almost everyone the first time.

## What Makes Braket Unique: Hardware Comparison

The same circuit (with minor adaptation) runs on completely different qubit technologies:

```python
# IonQ Aria - trapped-ion qubits
ionq = AwsDevice("arn:aws:braket:us-east-1::device/qpu/ionq/Aria-1")

# Rigetti Ankaa - superconducting qubits
rigetti = AwsDevice("arn:aws:braket:us-west-1::device/qpu/rigetti/Ankaa-3")
```

| | Superconducting (IBM/Rigetti) | Trapped ion (IonQ) |
|---|---|---|
| Gate speed | ~$50$ ns | ~$100$ μs |
| Coherence time | ~$100$ μs | ~seconds |
| All-to-all connectivity | No | Yes |
| Gate error per [[CNOT]] | ~$0.3$–$0.5$% | ~$0.3$% |

Running the same Bell state on both and comparing histograms teaches more about quantum hardware than any textbook. The distributions will differ noticeably.

**Warning on QPU cost:** IonQ charges $\$0.30$/task + $\$0.08$/shot. $1000$ shots = $\$80.30$. Test entirely on simulators first. Set AWS billing alerts.

## QuEra Aquila: The Analog Exception

QuEra's Aquila device is **not a gate-based computer**. It uses neutral atoms and programs via laser pulses and atom positions - an `AnalogHamiltonianSimulation` object, not a `Circuit`. If you see Aquila on the device list, it requires a completely different programming model.

## Sources
- [Amazon Braket getting started](https://docs.aws.amazon.com/braket/latest/developerguide/braket-get-started.html)
- [Hello AHS (QuEra)](https://docs.aws.amazon.com/braket/latest/developerguide/braket-get-started-hello-ahs.html)
- [Braket SDK GitHub examples](https://github.com/amazon-braket/amazon-braket-examples)
- [Amazon Braket pricing](https://aws.amazon.com/braket/pricing/)

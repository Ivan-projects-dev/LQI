#SoftDev #Python 
**Amazon Braket** (aws.amazon.com/braket) is AWS managed quantum computing service. Provides unified access to multiple QPU providers, $3$ managed simulators, & local simulator - all through single Python SDK. Integrated with the broader AWS ecosystem ($S3$ for results, IAM for access control, CloudWatch for monitoring).
- **$1$ free hour of on-demand simulator time per month** for $1st$ **$12$ months** after account creation (AWS Free Tier)
- **Local simulator** in the Braket SDK is always free (runs on your own machine)
- **Amazon Braket notebooks** (Jupiter) - $1$ free hour/month for $1st$ year
- QPU time is **always paid** - no free hardware credits by default

| Simulator                   | Type                       | Max [[Qubits]] | Cost (after free tier) |
| --------------------------- | -------------------------- | -------------- | ---------------------- |
| **$SV_1$** (State Vector)   | Exact statevector          | $34$           | $0.075/min             |
| **$DM_1$** (Density Matrix) | Noisy circuits             | $17$           | $0.075/min             |
| **$TN_1$** (Tensor Network) | Sparse/structured circuits | $50$           | $0.275/min             |
| **Local**                   | Statevector (no cloud)     | $~25$          | Free                   |

| Hardware Provider | Technology                   | Pricing (per task + per shot) |
| ----------------- | ---------------------------- | ----------------------------- |
| **[[IonQ]] Forte**    | Trapped-ion, $36$ [[Qubits]] | $0.30 + $0.08/shot            |
| **[[IonQ]] Aria**     | Trapped-ion, $25$ [[Qubits]] | $0.30 + $0.03/shot            |
| **[[Rigetti]] Ankaa** | Superconducting              | $0.30 + $0.00090/shot         |
| **IQM Garnet**    | Superconducting              | $0.30 + $0.00145/shot         |
| **QuEra Aquila**  | Neutral-atom (analog)        | $0.30 + $0.01/shot            |

SDK: `amazon-braket-sdk`. Circuits use `Circuit` objects; analog devices use `AnalogHamiltonianSimulation`.
```python
import boto3
from braket.aws import AwsDevice
from braket.circuits import Circuit

device = AwsDevice("arn:aws:braket:::device/quantum-simulator/amazon/sv1")

bell = Circuit().h(0).cnot(0, 1)
task = device.run(bell, shots=1000, s3_destination_folder=("my-bucket", "results"))
result = task.result()
print(result.measurement_counts)
```
Also supports **Qiskit**, **Cirq**, & **[[OpenQASM]] 3** circuits via the Braket SDK transpilers.

**Amazon Braket Hybrid Jobs** runs iterative quantum-classical algorithms (like [[VQE]], [[QAOA]]) as managed AWS jobs - classical compute runs on $EC2$, quantum tasks are submitted automatically. Provides priority QPU access during job execution.

**Always start with LocalSimulator.** The local simulator runs on your machine, is completely free, & returns results instantly. It handles up to $~25$ [[Qubits]]. There is no reason to use cloud simulators during dev:
```python
from braket.devices import LocalSimulator
device = LocalSimulator()
task = device.run(circuit, shots=1000)
```

**$S3$ bucket is mandatory for QPU jobs.** Cloud QPU results are stored in $S3$ before being returned. Create a bucket in the same region as your Braket workspace, or jobs will fail silently:
```python
task = device.run(circuit, shots=100,
    s3_destination_folder=("my-braket-bucket", "results/experiment1"))
```

**$DM_1$ for realistic noise, TN1 for shallow wide circuits.** `SV1` is noiseless - use it to verify circuit [[Logic]]. `DM1` (Density [[Matrix]]) applies configurable depolarizing noise & gives hardware-representative results. `TN1` is efficient only for circuits with limited entanglement (e.g., $1D$ nearest-neighbor, MERA); deeply entangled circuits will time out.

**QPU costs accumulate quickly.** $1000$-shot [[IonQ]] Forte job costs $0.30 + 1000 × $0.08 = **$80.30**. Per-task fee is charged even if $0$ shots complete. Set AWS billing alerts before any hardware experiments.

**QuEra Aquila is not gate-based.** Aquila programs neutral-atom arrays by specifying atom positions & laser drive waveforms via `AnalogHamiltonianSimulation`. It is analog quantum simulator optimized for many-body physics problems, not circuit-model algorithms.

**Cross-provider comparison is Bracket's unique strength.** Same `Circuit` object (with minor adaptation) can run on [[IonQ]] trapped ions, [[Rigetti]] superconducting [[Qubits]], & IQM superconducting [[Qubits]]. Comparing error profiles, gate fidelities, & measurement distributions across hardware teaches intuitions that no single platform can.
## Sources
- [Amazon Braket](https://aws.amazon.com/braket/)
- [Amazon Braket pricing](https://aws.amazon.com/braket/pricing/)
- [Amazon Braket documentation](https://docs.aws.amazon.com/braket/)
- [Braket SDK GitHub](https://github.com/amazon-braket/amazon-braket-sdk-python)
- [Braket simulators overview](https://aws.amazon.com/blogs/quantum-computing/simulating-quantum-circuits-with-amazon-braket/)

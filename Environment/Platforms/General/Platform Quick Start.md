#SoftDev #Python #Hardware
How to go from zero to first running circuit on each platform. All Python implementations in `(Py).md` files use `default.qubit` & run with just [[PennyLane]] - no account needed. The steps below are for submitting to real hardware or cloud simulators.

---
## [[PennyLane]] - local, no account
Fastest path to running any `(Py).md` file.
```bash
pip install pennylane
python qpe_chemistry.py   # or any other (Py).md script saved as .py
```
To use GPU-accelerated simulation (faster for 20+ qubits):
```bash
pip install pennylane-lightning[gpu]   # needs CUDA GPU
# change device line in script:
dev = qml.device("lightning.gpu", wires=n)
```
No credentials, no quota, no queue. Works fully offline.

---
## [[IBM Quantum]] - real QPU + simulator
**1. Account**: sign up free at `quantum.cloud.ibm.com` (IBM ID). No credit card.
**2. Get API token**: Dashboard → user menu → "Copy token".
**3. Install**:
```bash
pip install qiskit qiskit-ibm-runtime
```
**4. Save credentials once**:
```python
from qiskit_ibm_runtime import QiskitRuntimeService
QiskitRuntimeService.save_account(token="YOUR_TOKEN_HERE", set_as_default=True)
```
**5. First real circuit**:
```python
from qiskit import QuantumCircuit
from qiskit_ibm_runtime import QiskitRuntimeService, SamplerV2

service = QiskitRuntimeService()
backend = service.least_busy(operational=True, simulator=False)

qc = QuantumCircuit(2)
qc.h(0); qc.cx(0, 1); qc.measure_all()

job = SamplerV2(backend).run([qc], shots=1000)
print(job.result()[0].data.meas.get_counts())
```
**Free quota**: $10$ min QPU per $28$-day window. Simulators unlimited.
**Test without spending quota** (use `FakeSherbrooke` before real submission):
```python
from qiskit_aer import AerSimulator
from qiskit_ibm_runtime.fake_provider import FakeSherbrooke
sim = AerSimulator.from_backend(FakeSherbrooke())
```
See [[IBM Quantum]], [[IBM Primitives]].

---
## [[PennyLane]] on IBM hardware
Run any PennyLane circuit on IBM QPU - no Qiskit rewrite needed:
```bash
pip install pennylane-qiskit
```
```python
import pennylane as qml
dev = qml.device("qiskit.ibmq", wires=2, backend="ibm_kingston")
# rest of circuit unchanged
```
Credentials must be saved via `QiskitRuntimeService.save_account(...)` first.

---
## [[Amazon Braket]] - multi-hardware cloud
**1. AWS account**: `aws.amazon.com` → create account (credit card required, but free tier covers $1$ hr/month simulator).
**2. Install**:
```bash
pip install amazon-braket-sdk boto3
```
**3. Configure AWS credentials**:
```bash
aws configure   # enter Access Key ID, Secret, region (us-west-2 for Braket)
```
Or set env vars: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`.
**4. Create S3 bucket** in `us-west-2` (required for cloud results) via AWS Console.
**5. First circuit (free - local simulator)**:
```python
from braket.devices import LocalSimulator
from braket.circuits import Circuit

device = LocalSimulator()
bell = Circuit().h(0).cnot(0, 1)
result = device.run(bell, shots=1000).result()
print(result.measurement_counts)
```
**First cloud circuit (uses free-tier SV1 quota)**:
```python
from braket.aws import AwsDevice

sv1 = AwsDevice("arn:aws:braket:::device/quantum-simulator/amazon/sv1")
task = sv1.run(bell, shots=1000, s3_destination_folder=("YOUR-BUCKET", "results/"))
print(task.result().measurement_counts)
```
See [[Amazon Braket]], [[S3]].

---
## [[Azure Quantum]] - Q# + Python
**1. Azure account**: `portal.azure.com` (free $200$ credit for new accounts).
**2. Create Quantum Workspace**: Azure Portal → search "Azure Quantum" → create workspace → select free [[Quantinuum]] & [[IonQ]] providers.
**3. Install**:
```bash
pip install azure-quantum qiskit-ionq   # Python path
# or install VS Code + QDK extension for Q#
```
**4. Submit from Python (Qiskit + Azure)**:
```python
from azure.quantum import Workspace
from azure.quantum.qiskit import AzureQuantumProvider
from qiskit import QuantumCircuit

workspace = Workspace(
    resource_id="/subscriptions/.../resourceGroups/.../providers/Microsoft.Quantum/Workspaces/...",
    location="eastus"
)
provider = AzureQuantumProvider(workspace)
backend = provider.get_backend("quantinuum.sim.h1-1e")  # free emulator

qc = QuantumCircuit(2); qc.h(0); qc.cx(0, 1); qc.measure_all()
job = backend.run(qc, shots=100)
print(job.result().get_counts())
```
**Free path**: use the Quantinuum H1-1 Emulator (free, no HQT credits needed) at `quantum.microsoft.com` → Copilot, or in VS Code with QDK.
See [[Azure Quantum]], [[Quantum Katas]].

---
## [[D-Wave]] Leap - quantum annealing
**1. Account**: `cloud.dwavesys.com` → sign up for free Leap account.
**2. Install**:
```bash
pip install dwave-ocean-sdk
dwave setup   # interactive: paste your API token from cloud.dwavesys.com
```
**3. First QUBO (Max-Cut on triangle)**:
```python
import dimod
from dwave.system import EmbeddingComposite, DWaveSampler

# Max-cut on triangle: maximize cuts (equivalent to QUBO)
Q = {(0,0): -1, (1,1): -1, (2,2): -1,
     (0,1):  2, (1,2):  2, (0,2):  2}

sampler = EmbeddingComposite(DWaveSampler())
response = sampler.sample_qubo(Q, num_reads=100)
print(response.first.sample)   # e.g., {0:1, 1:0, 2:1}
```
**Free quota**: Leap free tier gives limited QPU seconds per month. Use `LeapHybridSampler` for larger problems (more generous quota).
See [[D-Wave]], [[QUBO Constraints]].

---
## [[qBraid]] - browser-based, no local install
**1**: go to `lab.qbraid.com` → sign up free.
**2**: open JupyterLab → select a pre-configured environment (Qiskit, PennyLane, Braket, etc.).
**3**: all SDKs pre-installed - no `pip install` needed, just `import`.
**4**: run any `(Py).md` code directly in notebook.

Free tier: unlimited JupyterLab sessions, no QPU credits included. Best for trying code without any local Python setup. See [[qBraid]].

---
## Which platform for which file

| Script | Best platform | Why |
|--------|--------------|-----|
| `qpe_chemistry.py` | [[PennyLane]] local or [[qBraid]] | Simulator only, no account needed |
| `qpe_ising.py` | [[PennyLane]] local or [[D-Wave]] ([[QUBO]] reformulation) | Simulator; [[D-Wave]] for annealing version |
| `qpe_walk.py` | [[PennyLane]] local or IBM FakeBackend | Simulator only |
| [[QPE]] on real hardware | [[IBM Quantum]] (ibm_kingston) | Free QPU access, 10 min/month |
| Full [[Shor]] factoring | Azure [[QRE]] first | Understand resource cost before hardware |

Source: [Qiskit documentation](https://docs.quantum.ibm.com/) [Amazon Braket documentation](https://docs.aws.amazon.com/braket/) [Azure Quantum — MS Learn](https://learn.microsoft.com/en-us/azure/quantum/) [D-Wave Ocean SDK](https://docs.ocean.dwavesys.com/) [PennyLane documentation](https://docs.pennylane.ai/)

#SoftDev #Hardware #Python 
**IBM Quantum** (`quantum.cloud.ibm.com`) is IBM's cloud quantum computing service, offering free public access to real superconducting QPUs & the **Qiskit** open-source SDK. $1$ of the most widely used quantum platforms for research & education.
Plans:
- **Open (free)** - $10$ min QPU per $28$-day rolling window; active users who log $20$ min in any $12$-month period can opt into a one-time $180$-min promotion for the next $12$ months (as of March $2026$). No credit card required; sign up with IBM ID. Full access to all on-demand simulators.
- **Flex** - pre-purchase at least $400$ minutes (usable within $1$ year). Minutes allocated to instances. Good for predictable usage between $400$-$10000$ min/year.
- **Pay-As-You-Go** - pay only for QPU time consumed. Best for flexible usage below $400$ min/year.
- **Premium** - enterprise subscription. Unlocks Qiskit Functions (higher-level algorithm primitives). Contact IBM.
- **On-Prem** - dedicated on-premises quantum system serviced by IBM Quantum. For organizations needing high data control.
- Free courses & learning resources on IBM Quantum Learning

IBM's fleet uses **superconducting transmon [[Qubits]]** arranged in heavy-hex lattice topology. Available systems:
- Multiple systems publicly accessible; flagship open-access device is **Heron r2 ibm_kingston** ($156$ [[Qubits]], $\sim 2\times10^{-3}$ median $2$-qubit error, $340$k CLOPS)
- **Eagle** ($127$ [[Qubits]]), **Heron** ($133$–$156$ [[Qubits]], higher fidelity), & smaller systems for experimentation
- Systems listed at `quantum.cloud.ibm.com` with live calibration data (gate error rates, $T_1/T_2$, readout fidelity)

Primary SDK: **Qiskit** (Python). Circuits are written as `QuantumCircuit` objects & compiled with `transpile()` before submission via the **Qiskit Runtime** service.
```python
from qiskit import QuantumCircuit
from qiskit_ibm_runtime import QiskitRuntimeService, SamplerV2

service = QiskitRuntimeService()
backend = service.least_busy(operational=True, simulator=False)

qc = QuantumCircuit(2, 2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()
sampler = SamplerV2(backend)
job = sampler.run([qc], shots=1024)
result = job.result()
```

**Primitives** abstract QPU access into $2$ interfaces:
- `SamplerV2` - returns shot-level measurement distributions
- `EstimatorV2` - returns expectation values of Pauli observables (used in [[VQE]], [[QAOA]])
### Simulators
- **ibmq_qasm_simulator** - state [[Vector]] & stabilizer sim, up to $32$ [[Qubits]]
- **AerSimulator** (local, via `qiskit-aer`) - noise model simulation using real device calibration data
- **FakeBackends** - drop-in replacements for real devices with realistic noise models for offline testing
### Ecosystem
- **Qiskit** - circuit composition, transpilation, optimization passes
- **Qiskit Runtime** - managed execution env on IBM Cloud
- **IBM Quantum Learning** - free courses, tutorials, Jupyter notebooks
- **Qiskit Patterns** - templates for mapping real-world problems to quantum circuits

Source: [Qiskit documentation](https://docs.quantum.ibm.com/) [IBM Quantum Platform](https://quantum.cloud.ibm.com/)

**Transpilation depth explosion** is the most common surprise. IBM's native gate set is `{ECR, RZ, SX, X}`. A [[CNOT]] becomes `ECR` + $2$ `SX` rotations. $10$-gate circuit can compile to $40+$ native gates. Always check:
```python
from qiskit import transpile
tqc = transpile(qc, backend, optimization_level=3)
print(tqc.depth())   # may be 3-10× your original depth
```
Use `optimization_level=3` for best compression at the cost of compile time.

**FakeBackends for offline noise testing** - avoid spending QPU minutes on circuits that haven't been tested with realistic noise:
```python
from qiskit_aer import AerSimulator
from qiskit_ibm_runtime.fake_provider import FakeSherbrooke
noisy_sim = AerSimulator.from_backend(FakeSherbrooke())
```

**Qubit selection matters.** Calibration data (error rates, $T_1/T_2$) is on the backend dashboard. For $2$-qubit circuits, route to the qubit pair with the best [[CNOT]] error. Use `backend.properties()` to get live data programmatically.

**Queue times.** `least_busy()` helps, but real QPU queues during peak hours are $30$ min - $3$ hours. Submit, close the laptop, come back later. Use `job.status()` or IBM Quantum web dashboard to track.

**Qiskit $1.0$ breaking change.** `execute()` was removed. Any tutorial pre-$2024$ using `execute(circuit, backend)` needs to be rewritten with `SamplerV2` / `EstimatorV2`. This breaks most Stack Overflow answers.

**Measurement [[Error Mitigation]]** - readout error is often the dominant error on current hardware & is easy to mitigate. The `mthree` library applies [[Matrix]] inversion correction with min overhead.
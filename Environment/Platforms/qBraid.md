#Quantum #Cloud #Platform
**qBraid** (qbraid.com) is a platform-agnostic quantum computing environment that provides browser-based access to 34+ quantum devices from multiple providers, cross-framework SDK conversion, & a managed JupyterLab environment. Designed as a single hub for quantum dev without local installation.

## Free Tier

- **Free base plan** - unlimited access to qBraid Lab (hosted JupyterLab)
- Free access to **qBraid's on-demand quantum simulators**
- **Intel Quantum SDK** available free with no installation required
- QPU time on partner hardware requires purchasing **qBraid Credits**
- 30+ pre-configured environments in Python, Julia, C++, & Q# - no setup needed

## Supported Hardware (via credits)

Access to QPUs from: **IonQ**, **AQT**, **QuEra**, **Rigetti**, **PASQAL**, **IQM**, & more - all through a unified API. Hardware availability via AWS Braket & direct provider connections.

## Supported SDKs (18+ with cross-conversion)

Qiskit, Cirq, PennyLane, [[OpenQASM]] 2/3, QIR, PyQuil, Braket SDK, Q#, Strawberry Fields, tket (Pytket), CUDA-Q, & more. **qBraid Transpiler** converts circuits between any two supported frameworks automatically.

## qBraid SDK

```python
from qbraid.runtime import QbraidSession
from qbraid.transpiler import transpile

# Convert a Qiskit circuit to Cirq
cirq_circuit = transpile(qiskit_circuit, "cirq")

# Run on any provider via unified API
session = QbraidSession()
device = session.get_device("aws_sv_sim")
job = device.run(cirq_circuit, shots=1000)
result = job.result()
```

## qBraid Lab

Browser-based JupyterLab with:
- Pre-installed quantum SDKs (no `pip install` needed)
- GPU-accelerated simulators via PennyLane Lightning
- GitHub & VS Code integration
- Real-time collaboration
- Chat-based assistant for quantum coding help

## Practical Notes

**qBraid is best used as a multiplexer, not a primary dev environment.** Write & debug circuits in Qiskit, PennyLane, or Cirq natively. Use qBraid when you want to run the same circuit on multiple backends for comparison, or when you need access to a device not covered by your primary platform.

**The unified transpiler saves boilerplate.** Converting a Qiskit `QuantumCircuit` to Cirq `Circuit` or Braket `Circuit` manually is error-prone. `qbraid.transpiler.transpile(circuit, "braket")` handles gate mapping automatically - inspect the output before submitting to verify correctness.

**JupyterLab environment has preinstalled SDKs.** The qBraid cloud environment comes with Qiskit, Cirq, Braket, PennyLane, & others preconfigured. This is useful for short experiments without local setup overhead.

**Free tier is resource-limited.** The free plan limits compute time per session. For serious variational algorithm work ([[VQE]], [[QAOA]] optimization loops), local dev or a paid tier is more practical.
## Sources
- [qBraid Platform](https://www.qbraid.com)
- [qBraid Lab](https://www.qbraid.com/lab)
- [qBraid pricing](https://www.qbraid.com/pricing)
- [qBraid SDK GitHub](https://github.com/qBraid/qBraid)
- [qBraid software overview](https://www.qbraid.com/software)

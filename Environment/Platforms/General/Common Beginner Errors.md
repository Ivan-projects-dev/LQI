#Quantum #Beginner
Exact error messages, why they happen, and how to fix them. Collected from the most frequent stumbling points on each platform.

---

## IBM Quantum / Qiskit

### `AttributeError: 'QiskitRuntimeService' object has no attribute 'run'`
You're using an old workflow. `QiskitRuntimeService` doesn't run circuits directly. Use a **primitive** (`SamplerV2` or `EstimatorV2`):
```python
# WRONG (old API, removed in Qiskit 1.0)
job = service.run(qc, backend=backend)

# CORRECT
sampler = SamplerV2(backend)
job = sampler.run([transpile(qc, backend)], shots=1024)
```

### `IBMInputValueError: 'The instruction ... is not supported'`
Your circuit contains a gate the backend doesn't support natively. You forgot to transpile:
```python
# Always transpile before submitting
from qiskit import transpile
tqc = transpile(qc, backend, optimization_level=3)
sampler.run([tqc])   # submit the transpiled circuit, not the original
```

### `KeyError: 'meas'` when extracting counts
The measurement register name doesn't match. `measure_all()` creates a register named `meas`. If you used `measure(0, 0)` manually, the register name is whatever you named it (default `c`):
```python
# With measure_all():
counts = result[0].data.meas.get_counts()

# With QuantumCircuit(2, 2) + qc.measure([0,1], [0,1]):
counts = result[0].data.c.get_counts()   # register named 'c'

# Safe universal way:
pub = result[0]
reg_name = list(pub.data)[0]             # get whatever the register is called
counts = getattr(pub.data, reg_name).get_counts()
```

### Circuit runs but all results are `00000`
You forgot `measure_all()` or any measurement instruction. A circuit submitted without measurements returns all zeros. Add `qc.measure_all()` before transpiling.

### `QiskitRuntimeError: No IBM Quantum account found`
The API token isn't saved yet. Run this **once** to store it permanently:
```python
from qiskit_ibm_runtime import QiskitRuntimeService
QiskitRuntimeService.save_account(
    channel="ibm_quantum",
    token="YOUR_TOKEN_HERE",   # from quantum.cloud.ibm.com → Account settings
    overwrite=True
)
```
After saving, `QiskitRuntimeService()` works without arguments.

### `qc.draw('mpl')` crashes with `ModuleNotFoundError`
Matplotlib circuit diagrams need two extra packages:
```bash
pip install matplotlib pylatexenc
```

### Transpiled circuit is much deeper than expected
`optimization_level=0` does minimal rewriting. Always use level 3 for real hardware:
```python
tqc = transpile(qc, backend, optimization_level=3)
print(f"Original depth: {qc.depth()}, Transpiled: {tqc.depth()}")
```
A 5-gate circuit at level 0 may become 30 gates; at level 3 it may stay at 8. The difference matters enormously on noisy hardware.

---

## Azure Quantum / Q#

### `Unresolved symbol: Std.Arrays`
The QDK project file is missing or the import is wrong. Two fixes:

**Fix 1 — Add a `qsharp.json` project file** in the same folder as your `.qs` file:
```json
{
  "author": "you",
  "license": "MIT"
}
```
The QDK extension needs this file to recognize a Q# project.

**Fix 2 — Check import syntax.** Q# 1.0 uses `import`, not `open`:
```csharp
// WRONG (pre-1.0 syntax)
open Microsoft.Quantum.Arrays;

// CORRECT (Q# 1.0)
import Std.Arrays.*;
```

### `Runtime error: Released qubits are not in the ground state`
You measured a qubit but didn't reset it before the `use` block ended. Always reset:
```csharp
use q = Qubit();
H(q);
let r = M(q);      // measures, but q may now be |1⟩
// WRONG: block ends here with q in unknown state

// CORRECT: use MResetZ which measures AND resets
let r = MResetZ(q);

// OR: explicitly reset
let r = M(q);
Reset(q);
```

### `Error: Operation 'MyOp' does not support the Adjoint functor`
The operation inside your `within` block or called with `Adjoint` doesn't have `is Adj` in its signature. Every operation used in `within/apply` must support adjoint:
```csharp
// WRONG: missing is Adj
operation MyOp(q : Qubit) : Unit {
    H(q);
}

// CORRECT
operation MyOp(q : Qubit) : Unit is Adj + Ctl {
    H(q);
    // adjoint auto; is inferred for simple cases
}
```
If your operation contains a measurement (`M()`), it cannot be `Adj` — measurements are irreversible.

### `DumpMachine()` output doesn't appear
Output goes to the **Debug Console** in VS Code, not the Terminal. Open it with `Ctrl+Shift+Y` (Windows/Linux) or `Cmd+Shift+Y` (Mac). `Message()` output also goes there.

### The QDK language server takes 60+ seconds to start
Normal on first launch. The extension builds the Q# compiler in the background. Wait for the status bar at the bottom of VS Code to stop showing "Q# Loading". Subsequent launches are faster.

### `@Config(AdaptiveRI)` operation fails to compile for Base profile
You're trying to run an adaptive operation on a target that only supports `Base`. Either:
- Switch to `Unrestricted` profile for simulation
- Provide a `@Config(Base)` variant of the same operation (see [[Adaptive profile]])

---

## PennyLane

### `ValueError: differentiation method 'backprop' not supported on this device`
`backprop` requires access to the full statevector — only works on `default.qubit` and similar simulators. Switch to `parameter-shift` for hardware or shot-based backends:
```python
# WRONG on shot-based device
@qml.qnode(dev, diff_method="backprop")

# CORRECT for hardware or shots > 0
@qml.qnode(dev, diff_method="parameter-shift")
```

### Gradient is always zero
Two common causes:

**Cause 1:** The measurement observable commutes with all your gates — the expectation value is flat everywhere. Try a different observable or different gate types.

**Cause 2:** Using `numpy` instead of `pennylane.numpy` (PennyLane's autodiff-aware wrapper):
```python
import numpy as np              # WRONG — no gradient tracking
from pennylane import numpy as np  # CORRECT
```

### Circuit runs but `qml.grad` fails with `TypeError`
The parameter must have `requires_grad=True` when using PennyLane's built-in gradient:
```python
from pennylane import numpy as np
theta = np.array(0.5, requires_grad=True)   # required flag
grad = qml.grad(circuit)(theta)
```
With PyTorch or JAX, use their native gradient tools instead of `qml.grad`.

### `Device not found: 'lightning.gpu'`
Lightning GPU requires separate installation:
```bash
pip install pennylane-lightning[gpu]
# also needs CUDA toolkit installed
```
Fall back to `lightning.qubit` (CPU, still faster than `default.qubit`) if GPU unavailable.

---

## Amazon Braket

### `ValueError: S3 bucket ... does not exist`
You must create an S3 bucket before submitting cloud or QPU tasks. The bucket name must be unique globally and exist in the same AWS region as your Braket workspace:
```python
task = device.run(circuit, shots=100,
    s3_destination_folder=("my-braket-results-bucket", "subfolder/"))
#                          ^ bucket must exist in AWS S3
```
Create the bucket in the AWS console → S3 → Create bucket → choose your region.

### Task shows as `COMPLETED` but `task.result()` hangs
The result is being downloaded from S3. If the task just finished, wait 5-10 seconds and retry. For large results (many shots, many qubits), the S3 download takes time. Add a short delay:
```python
import time
while task.state() != "COMPLETED":
    time.sleep(2)
result = task.result()
```

### `ClientError: An error occurred (ValidationException)`
Usually means the circuit contains a gate the selected device doesn't support. Check the device's supported gate set:
```python
device = AwsDevice("arn:aws:braket:...")
print(device.properties.action['braket.ir.openqasm.program'].supportedOperations)
```

### Free tier simulator ran out
The AWS free tier gives 1 hour of `SV1`/`DM1`/`TN1` time per month. After exhaustion, each minute costs $0.075. The **local simulator** is always free and has no quota — use it for development:
```python
from braket.devices import LocalSimulator
device = LocalSimulator()   # never charges, never runs out
```

---

## D-Wave Leap

### `SolverNotFoundError` or authentication failure
The config wasn't created or has a wrong token. Re-run the setup and verify:
```bash
dwave config create    # interactive: enter API token from cloud.dwavesys.com
dwave ping             # should print: "PING ... QPU online"
```

### Solution quality is poor / random-looking
Usually a chain strength problem. Check `chain_break_fraction` in the response:
```python
print(response.first.chain_break_fraction)   # should be < 0.05
```
If it's high (> 0.1), increase chain strength:
```python
response = sampler.sample_ising(h, J, num_reads=100, chain_strength=2.0)
# default is usually 1.0 — try 1.5, 2.0, 3.0
```

### `ValueError: Problem is too large for available QPU`
The problem graph can't be embedded on the Pegasus topology with the available physical qubits. Options:
1. Reduce problem size
2. Use `LeapHybridSampler` (no size limit):
```python
from dwave.system import LeapHybridSampler
sampler = LeapHybridSampler()   # handles arbitrarily large BQMs
```

### Embedding takes minutes
`EmbeddingComposite` computes the embedding fresh each call. Cache it for repeated runs on the same problem structure:
```python
from dwave.system import DWaveSampler, FixedEmbeddingComposite
from minorminer import find_embedding

sampler = DWaveSampler()
embedding = find_embedding(Q, sampler.edgelist)
cached = FixedEmbeddingComposite(sampler, embedding)
# now cached.sample_qubo(Q) reuses the same embedding every time
```

## Sources
- [Qiskit migration guide (0.x → 1.0)](https://docs.quantum.ibm.com/migration-guides/qiskit-1.0-features)
- [Q# language specification](https://learn.microsoft.com/en-us/azure/quantum/user-guide/)
- [PennyLane troubleshooting](https://pennylane.ai/faq/)
- [Amazon Braket troubleshooting](https://docs.aws.amazon.com/braket/latest/developerguide/braket-troubleshooting.html)
- [D-Wave Ocean SDK getting started](https://docs.ocean.dwavesys.com/en/stable/getting_started.html)

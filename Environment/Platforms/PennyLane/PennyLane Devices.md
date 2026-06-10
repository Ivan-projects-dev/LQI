#SoftDev #Python #CPP
Device choice in [[PennyLane]] determines what gradient methods work, what measurements are available, & whether JIT compilation is possible. Getting this wrong causes silent errors or missing features.
### Core Built-In Devices
**`default.qubit`** - Python statevector simulator. Most featureful device; supports every feature [[PennyLane]] offers.
- Supports `diff_method="backprop"` (fastest for small circuits, autograd/JAX/PyTorch)
- Supports `qml.state()` (full statevector access)
- Supports all measurement types
- Slow for $> 20$ [[Qubits]] - pure Python, no optimized backend
- Use for: learning, prototyping, debugging, variational circuits on small qubit counts

**`lightning.qubit`** - C++ statevector simulator. Drop-in replacement for `default.qubit` for larger circuits.
- $10$–$100\times$ faster than `default.qubit` for $20+$ qubit circuits
- Required for `@qml.qjit` JIT compilation (Catalyst)
- Does **not** support `diff_method="backprop"` - use `diff_method="adjoint"` instead (equally fast, hardware-compatible)
- Use for: performance-sensitive simulation, large circuits, any workflow using `@qjit`

**`default.mixed`** - mixed-state simulator. Tracks full density [[Matrix]] including noise.
- Required when explicitly modeling quantum noise (depolarizing, dephasing, amplitude damping)
- Compatible with ML frameworks (JAX, PyTorch) for differentiable noisy circuits
- Exponentially more memory than statevector ($4^n$ vs $2^n$) - practical limit $\sim 12$ [[Qubits]]
- Use for: noise modeling, open quantum systems, benchmarking noise mitigation
### Gradient Method by Device
| Device | `backprop` | `adjoint` | `parameter-shift` |
|---|---|---|---|
| `default.qubit` | ✓ | ✓ | ✓ |
| `lightning.qubit` | ✗ | ✓ (fast) | ✓ |
| `default.mixed` | ✗ | ✗ | ✓ |
| Hardware / shot-based | ✗ | ✗ | ✓ only |

**`backprop` fails on any device that is not `default.qubit`.** If you switch device mid-experiment without changing `diff_method`, training silently breaks - gradients may be zero or raise `ValueError`.

### Hardware Devices
Hardware backends require additional plugin packages:
```python
# IBM hardware via Qiskit
# pip install pennylane-qiskit
dev = qml.device("qiskit.remote", wires=5, backend=ibm_backend)

# Amazon Braket
# pip install amazon-braket-pennylane-plugin
dev = qml.device("braket.aws.qubit", device_arn="...", wires=5, s3_destination_folder=(...))
```
All hardware devices are shot-based. Only `parameter-shift` works. `qml.state()` raises an error.
### Selecting the Right Device
```python
dev = qml.device("default.qubit", wires=n) # Prototyping & debugging (any size, all features)
dev = qml.device("lightning.qubit", wires=n) # Fast simulation 20+ qubits or JIT
dev = qml.device("default.mixed", wires=n) # Noise-aware simulation
```
**Do not switch device without reviewing `diff_method` & measurement return types** - they are the most common source of silent breakage when porting circuit from simulator to hardware.

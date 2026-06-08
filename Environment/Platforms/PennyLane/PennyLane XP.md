#SoftDev #Python #ML
`pip install [[PennyLane]]`. `default.qubit` simulator works immediately with no credentials. Core pattern - `@qml.qnode`, return `qml.expval(...)`, call `qml.grad(circuit)(params)` - is clean & Pythonic. If you know PyTorch, the mental model transfers directly: circuit = forward pass, `qml.grad` = backward pass. $1st$ working training loop takes $~20$ lines.

**`requires_grad=True` must be set explicitly.** Plain Python float or standard numpy array passed as circuit parameter gives $0$ gradient. Must be:
```python
from pennylane import numpy as np
theta = np.array(0.5, requires_grad=True)
```

**`backprop` only works on `default.qubit`.** If you switch to a shot-based device or any hardware backend, `diff_method="backprop"` raises `ValueError`. Use `parameter-shift` for hardware - it runs the circuit twice per parameter but is hardware-compatible.

**Return type of QNode determines what you get.** `qml.expval(op)` returns a single float. `qml.probs(wires=[0,1])` returns a probability array. `qml.state()` returns the full statevector. `qml.state()` only works on `default.qubit` - not on shot-based or hardware devices. Calling it on a shot device raises an error.

**Expectation values on hardware are noisy at low shot counts.** With `shots=100`, gradient estimates fluctuate by $\pm 0.05$. This breaks gradient descent. Use `shots ≥ 512` for training, `shots ≥ 4096` for final evaluation.

### Challenges
**Barren plateaus.** For deep ($> 10$ layers), wide ($> 8$ [[Qubits]]) parameterized circuits, gradient variance vanishes exponentially. Optimization becomes impossible - every gradient estimate is noise. Mitigations: local cost funcs (measure few [[Qubits]] only), layer wise training, identity-block initialization. See [[PennyLane Variational Training]].

**`qml.state()` mid-circuit inspection is awkward.** You cannot check state at an intermediate point without restructuring the circuit into $2$ separate QNodes. There is no `save_statevector` equivalent. Plan the circuit before building, verify against known inputs at boundary parameter values ($0$, $\pi/2$, $\pi$).

**`lightning.qubit` required for JIT.** `@qml.qjit` compiles the training loop to native code ($10$–$100\times$ speedup), but requires `lightning.qubit` or `lightning.gpu` - not `default.qubit`. `pip install [[PennyLane]]-lightning` separately. First JIT call takes $1$–$5$ seconds to compile; subsequent calls are fast. Parameters must have fixed shapes.

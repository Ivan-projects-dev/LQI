#SoftDev #Python 
###   `ValueError: differentiation method 'backprop' not supported on this device`
`backprop` requires access to the full statevector - only works on `default.qubit` & similar simulators. Switch to `parameter-shift` for hardware or shot-based backends:
```python
# WRONG on shot-based device
@qml.qnode(dev, diff_method="backprop")

# CORRECT for hardware or shots > 0
@qml.qnode(dev, diff_method="parameter-shift")
```
###  Gradient is always $0$
- **Cause 1:** The measurement observable commutes with all your gates - the expectation value is flat everywhere. Try a different observable or different gate types.
- **Cause 2:** Using `numpy` instead of [[PennyLane]]`.numpy` ([[PennyLane]]'s autodiff-aware wrapper):
```python
import numpy as np # WRONG - no gradient tracking
from pennylane import numpy as np # CORRECT
```
###  Circuit runs but `qml.grad` fails with `TypeError`
Parameter must have `requires_grad=True` when using [[PennyLane]]'s built-in gradient:
```python
from pennylane import numpy as np
theta = np.array(0.5, requires_grad=True) # required flag
grad = qml.grad(circuit)(theta)
```
With PyTorch or JAX, use their native gradient tools instead of `qml.grad`.
###  `Device not found: 'lightning.gpu'`
Lightning GPU requires separate installation:
```bash
pip install pennylane-lightning[gpu]
# also needs CUDA toolkit installed
```
Fall back to `lightning.qubit` (CPU, still faster than `default.qubit`) if GPU unavailable.
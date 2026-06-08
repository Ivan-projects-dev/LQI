#SoftDev #Python 
[[PennyLane]] debugging splits cleanly into $2$ types: **circuit logic** (is the [[Quantum state]] what I think it is?) & **gradient** (why is optimization not moving?). Treat them separately.
### Inspecting Intermediate State
`qml.state()` returns the full statevector. Only works on `default.qubit` - not on shot-based or hardware devices.
```python
import pennylane as qml

dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def debug_circuit():
    qml.Hadamard(wires=0)
    return qml.state()

print(debug_circuit())
# [0.707+0j, 0, 0.707+0j, 0]  →  (|00⟩ + |10⟩)/√2
```
To inspect state mid-circuit without changing the return type, split into $2$ separate QNodes - one that stops at the gate you want to inspect.
### Verifying Circuit Behavior With Known Inputs
Before attaching any gradient loop, verify the circuit produces correct outputs at known parameter values:
```python
@qml.qnode(dev)
def circuit(theta):
    qml.RY(theta, wires=0)
    return qml.expval(qml.PauliZ(0))

import numpy as np
print(circuit(0.0))       # should be  1.0  (qubit at |0⟩, Z = +1)
print(circuit(np.pi))     # should be -1.0  (qubit at |1⟩, Z = -1)
print(circuit(np.pi / 2)) # should be  0.0  (superposition, Z = 0)
```
If these are wrong, the circuit is wrong - fix it before touching optimization.
### Inspecting Circuit Structure
`qml.draw()` & `qml.specs()` expose what the circuit actually contains - useful when the circuit is built programmatically and may not be what you think:
```python
print(qml.draw(circuit)(0.5))     # ASCII diagram with parameter substituted
print(qml.specs(circuit)(0.5))    # gate counts, depth, num_params
```
If gate count or depth is unexpected, the circuit construction loop has a bug.

#SoftDev #Python 
Intermediate State with `qml.state()`
```python
import pennylane as qml
import numpy as np

dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def debug_circuit():
    qml.Hadamard(wires=0)
    # check state here
    return qml.state()   # returns full statevector

print(debug_circuit()) # [0.707+0j, 0, 0.707+0j, 0] = (|00⟩ + |10⟩)/√2
```
`qml.state()` returns the full statevector as NumPy array. You can only call this on `default.qubit` - not on shot-based or hardware devices.
### #Debugging Gradients
If your gradient is $0$ when it should not be:
```python
@qml.qnode(dev, diff_method="parameter-shift")
def circuit(theta):
    qml.RX(theta, wires=0)
    return qml.expval(qml.PauliZ(0))

theta = np.array(0.5, requires_grad=True)
grad = qml.grad(circuit)(theta)
print(grad)  # should be ~ -0.479
```
Common causes of $0$ gradient:
1. `theta` is plain Python float, not `np.array(..., requires_grad=True)`
2. Using `import numpy as np` instead of [[PennyLane]]'s `np` - `from` [[PennyLane]] `import numpy as np`
3. Expectation value is $0$ at the current parameters & the gradient is genuinely $0$ at that point - shift slightly & try again
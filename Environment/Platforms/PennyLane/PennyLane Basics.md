#Quantum #Beginner #QML #Pyhton
Concrete walkthrough of starting with [[Xanadu PennyLane]] for quantum machine learning.

**PennyLane** treats quantum circuits the way PyTorch treats neural networks: as differentiable functions you can optimize with gradient descent.

If you've used PyTorch or JAX, PennyLane will feel familiar. If you haven't, learn those basics first - PennyLane makes most sense as a quantum extension of classical autodiff.

### $2$ Core Concepts

**`qml.device`** - where the circuit runs (simulator or hardware):
```python
import pennylane as qml
dev = qml.device("default.qubit", wires=2)   # CPU simulator, 2 qubits
```

**`@qml.qnode`** - turns a Python function into a quantum circuit:
```python
@qml.qnode(dev)
def circuit(theta):
    qml.RX(theta, wires=0)       # rotation gate with tunable angle theta
    qml.CNOT(wires=[0, 1])
    return qml.expval(qml.PauliZ(0))   # expected value of Pauli Z on qubit 0
```

Call it like a normal function:
```python
import numpy as np
result = circuit(np.pi / 4)    # returns a float: the expectation value
print(result)                  # e.g., 0.707
```

## What Makes PennyLane Different: Gradients

You can compute the [[Derivative]] of the circuit output with respect to any parameter:

```python
grad_fn = qml.grad(circuit)
gradient = grad_fn(np.pi / 4)   # d(circuit)/d(theta) at theta = pi/4
print(gradient)
```

This uses the **parameter-shift rule** - it runs the circuit twice (at `theta + pi/2` and `theta - pi/2`) and computes the exact gradient from the difference. It works on real quantum hardware, not just simulators.

## Min Variational Algorithm
```python
import pennylane as qml
from pennylane import numpy as np

dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def circuit(theta):
    qml.RY(theta, wires=0)
    return qml.expval(qml.PauliZ(0))

# Minimize the expectation value (find theta where <Z> is lowest)
theta = np.array(0.1, requires_grad=True)
opt = qml.GradientDescentOptimizer(stepsize=0.4)

for step in range(20):
    theta, cost = opt.step_and_cost(circuit, theta)
    print(f"Step {step}: theta={theta:.4f}, <Z>={cost:.4f}")

# After convergence: theta ≈ pi, <Z> ≈ -1 (qubit pointing south = |1⟩)
```

This is toy version of [[VQE]]: find the circuit parameters that minimize an energy function.

## Running the Same Circuit on Different Backends

Change only the `device` line - the circuit stays the same:

```python
# Local CPU - free, instant
dev = qml.device("default.qubit", wires=2)

# GPU simulation - 100x faster for 20+ qubits
dev = qml.device("lightning.gpu", wires=2)

# Real IBM hardware (requires qiskit account)
dev = qml.device("qiskit.ibmq", wires=2, backend="ibm_kyiv")
```

## Sources
- [PennyLane installation](https://pennylane.ai/install/)
- [PennyLane tutorial: Getting started](https://pennylane.ai/qml/demonstrations/tutorial_pennylane_intro/)
- [Parameter-shift rule](https://pennylane.ai/qml/glossary/parameter_shift/)
- [Variational quantum eigensolver demo](https://pennylane.ai/qml/demonstrations/tutorial_vqe/)

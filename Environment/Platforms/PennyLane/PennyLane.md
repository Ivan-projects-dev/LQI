#ML #Python #Algorithm #SoftDev 
Python library for **quantum ML**, variational algorithms, & hybrid quantum-classical computing. Framework-agnostic: works as frontend for Qiskit, Cirq, [[Amazon Braket]], [[Strawberry Fields]] etc.
```python
import pennylane as qml
import numpy as np

dev = qml.device("default.qubit", wires=2)

@qml.qnode(dev)
def circuit(theta):
    qml.RY(theta, wires=0)
    qml.CNOT(wires=[0, 1])
    return qml.expval(qml.PauliZ(0))

grad = qml.grad(circuit)
print(grad(np.pi / 4))   # parameter-shift gradient
```
Key features:
- **Parameter-shift rule** - exact gradient computation for hardware-compatible circuits
- **Automatic differentiation** - supports JAX, PyTorch, TensorFlow backends
- **Catalyst** - JIT compiler for PennyLane circuits (AOT compilation via MLIR/QIR)
- **PennyLane Lightning** - high-performance C++ state-[[Vector]] simulator with GPU support (Lightning-GPU, Lightning-Kokkos)
- **`qml.`[[QNN]]** - Torch/Keras wrappers for hybrid quantum-classical neural networks

Source: [PennyLane documentation](https://docs.pennylane.ai/) [PennyLane — GitHub](https://github.com/PennyLaneAI/pennylane)

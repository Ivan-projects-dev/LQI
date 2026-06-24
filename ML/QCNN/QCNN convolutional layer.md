#ML
**QCNN convolutional layer** is the quantum analogue of a classical convolutional layer. It applies the same **parameterized 2-qubit unitary** to every pair (or small group) of neighboring [[Qubits]], sharing weights across all positions — exactly mirroring the weight-sharing of a classical filter sliding over an image.

### Structure
For an $n$-qubit register, one convolutional layer applies a parameterized unitary $U(\theta)$ to each adjacent pair:
- **Even layer**: pairs $(q_0, q_1)$, $(q_2, q_3)$, $(q_4, q_5)$, ...
- **Odd layer**: pairs $(q_1, q_2)$, $(q_3, q_4)$, $(q_5, q_6)$, ...

Alternating even/odd layers gives each qubit's neighborhood full coverage, matching the receptive field of a sliding window.

The unitary $U(\theta)$ is typically a **2-qubit ansatz block**: a sequence of single-qubit rotations $R_y(\theta_i)$, $R_z(\theta_i)$ followed by a [[CNOT]] or $CZ$ entangling gate. All pairs in the same layer share the **same $\theta$** — this is what makes it "convolutional" (translationally invariant).

```python
import pennylane as qml
from pennylane import numpy as np

dev = qml.device("default.qubit", wires=8)

def conv_layer(params, wires, start=0):
    # Apply 2-qubit block to all pairs with stride 2, starting at 'start'
    for i in range(start, len(wires) - 1, 2):
        qml.RY(params[0], wires=wires[i])
        qml.RY(params[1], wires=wires[i+1])
        qml.CNOT(wires=[wires[i], wires[i+1]])
        qml.RY(params[2], wires=wires[i])
        qml.RY(params[3], wires=wires[i+1])

@qml.qnode(dev)
def qcnn_circuit(params_conv1, params_conv2):
    # Encoding (amplitude or angle)
    for i in range(8):
        qml.Hadamard(wires=i)

    # Conv layer 1: even pairs
    conv_layer(params_conv1, wires=range(8), start=0)
    # Conv layer 1: odd pairs
    conv_layer(params_conv1, wires=range(8), start=1)

    # Pooling: discard every other qubit (measure qubits 1,3,5,7)
    # Then conv layer 2 on remaining 4 qubits
    conv_layer(params_conv2, wires=[0, 2, 4, 6], start=0)

    return qml.expval(qml.PauliZ(0))
```

### Why weight sharing matters
A classical $n$-pixel image with filter size $k$ uses $k^2$ weights regardless of image size. Without weight sharing, a quantum circuit on $n$ qubits would need $O(n)$ independent parameters per layer — scaling poorly and causing [[PennyLane Variational Training#The Barren Plateau Problem|barren plateaus]]. Weight sharing keeps parameter count constant per layer, which is key to training [[QCNN]]s on larger inputs.

### Expressivity vs. depth
- **Shallow conv layers** (1–2 rounds of even/odd pairs): low expressivity, fast training, small barren plateau risk
- **Deep conv layers** (many alternating rounds): higher expressivity, but circuit depth grows and noise accumulates on hardware

In practice, $1$–$2$ alternating conv rounds per "convolutional block" before a [[QCNN pooling layers|pooling layer]] gives a good trade-off.

### Classical analogy
| Classical CNN | QCNN |
|---|---|
| $k \times k$ filter | $2$-qubit unitary $U(\theta)$ |
| Sliding window | Alternating even/odd pair application |
| Shared weights | Shared $\theta$ across all pairs |
| ReLU activation | Measurement + classical post-processing |
| Max pooling | Qubit discard or measurement-based pooling |

Source: [Cong, Choi, Lukin — "Quantum convolutional neural networks" (2019)](https://arxiv.org/abs/1810.03912) [PennyLane QCNN demo](https://pennylane.ai/qml/demos/tutorial_quanvolution/)

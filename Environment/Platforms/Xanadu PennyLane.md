#Quantum #Cloud #Platform #QML #Photonic
**Xanadu** (xanadu.ai) is a Canadian quantum computing company focused on **photonic quantum hardware** & open-source quantum machine learning software. Its two main open-source products - **PennyLane** & **Strawberry Fields** - are freely available & widely used for quantum ML research & photonic simulation.

## Free Tier

- **PennyLane** & **Strawberry Fields** are fully **open-source & free** (Apache 2.0 license)
- Both run locally with no account required
- Cloud simulators via **Xanadu Cloud** require account registration; hardware access is by request
- **PennyLane demos & tutorials** at pennylane.ai are free interactive learning resources

## PennyLane

Python library for **quantum machine learning**, variational algorithms, & hybrid quantum-classical computing. Framework-agnostic: works as a frontend for Qiskit, Cirq, [[Amazon Braket]], Strawberry Fields, & more via plugins.

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

## Strawberry Fields

Python library for **continuous-variable (CV) / photonic** quantum computing. Operates on optical modes rather than discrete [[Qubits]]; gates include Displacement, Squeezing, Beamsplitter, & Kerr operations.

```python
import strawberryfields as sf
from strawberryfields import ops

prog = sf.Program(2)
with prog.context as q:
    ops.Sgate(0.54) | q[0]
    ops.BSgate(0.43, 0.1) | (q[0], q[1])
    ops.MeasureFock() | q[0]

eng = sf.Engine("fock", backend_options={"cutoff_dim": 5})
result = eng.run(prog)
```

## Aurora Hardware (2025)

Xanadu launched **Aurora** in $2025$ - the world's first **scalable networked photonic quantum computer** ($12$ logical [[Qubits]]). Uses photonic chip modules connected via optical fiber, designed for modular scale-out. Access via Xanadu Cloud (by application).

## Practical Notes

**The device swap is the whole value.** Write your circuit once, benchmark it on `default.qubit` (noiseless CPU), `lightning.gpu` (fast simulation), & then on a real backend (`qiskit.ibmq`, `braket.aws.qubit`). PennyLane's circuit runs identically on all. This is fastest way to understand hardware noise.

**Parameter-shift gradients work on real hardware; backprop does not.** `diff_method="backprop"` directly differentiates through the statevector - only possible on simulators. On any shot-based backend, use `"parameter-shift"` (exact) or `"finite-diff"` (approximate, $<$ circuit evaluations).

**Catalyst (`@qml.qjit`) for speed.** Variational optimization loops call the circuit hundreds or thousands of times. Without JIT compilation, each call has Python interpreter overhead. With `@qml.qjit`, the full optimization loop compiles to native code - typical speedup is 10–100×.

**Shot noise dominates variational optimization on hardware.** With `shots=1024` the gradient estimate has standard deviation $$\sim 1/\sqrt{N_{	ext{shots}}}$$. This makes convergence noisy. Increase shots progressively: coarse optimization with $512$ shots, fine-tuning with $4096+$.

**Strawberry Fields is a separate library.** Photonic (continuous-variable) quantum computing uses `strawberryfields.fock` or `strawberryfields.gaussian` devices. These have Fock states, displacement operators, & squeezing - completely different from `default.qubit`. The two paradigms do not mix.

## Sources
- [PennyLane](https://pennylane.ai)
- [Strawberry Fields](https://strawberryfields.ai)
- [PennyLane open-source platform blog (2025)](https://pennylane.ai/blog/2025/12/open-source-quantum-computing-pennyLane-catalyst-open-quantum-design)
- [Xanadu products](https://www.xanadu.ai/products/strawberry-fields/)
- [PennyLane GitHub](https://github.com/PennyLaneAI/pennylane)
- [Strawberry Fields GitHub](https://github.com/XanaduAI/strawberryfields)


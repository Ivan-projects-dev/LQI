#[[PennyLane]] #[[VQE]] #Experience #Training
What variational circuit training actually feels like - gradient descent on a quantum cost func, the problems you encounter, & the patterns that work.

## The Training Loop Pattern

Every variational algorithm ([[VQE]], [[QAOA]], QML) follows the same structure: a parameterized circuit, a cost func, & classical gradient descent. This is [[PennyLane]]'s home territory.

```python
import pennylane as qml
from pennylane import numpy as np

dev = qml.device("default.qubit", wires=4)

@qml.qnode(dev, diff_method="adjoint")   # fastest for local training
def ansatz(params):
    # Hardware-efficient ansatz: RY layers + CNOT entangling
    n_layers = params.shape[0]
    for layer in range(n_layers):
        for i in range(4):
            qml.RY(params[layer, i], wires=i)
        # Entangling layer
        qml.CNOT(wires=[0, 1]); qml.CNOT(wires=[1, 2]); qml.CNOT(wires=[2, 3])
    return qml.expval(qml.PauliZ(0) @ qml.PauliZ(1))   # cost observable

# Initialize parameters: small random values near zero
params = np.random.uniform(-0.1, 0.1, (3, 4), requires_grad=True)

# Adam optimizer (much better than vanilla gradient descent for VQE)
opt = qml.AdamOptimizer(stepsize=0.05)

losses = []
for step in range(200):
    params, loss = opt.step_and_cost(ansatz, params)
    losses.append(float(loss))
    if step % 20 == 0:
        print(f"Step {step:4d}: cost = {loss:.6f}")
```

**Why Adam, not SGD?** Vanilla gradient descent struggles with the curved loss landscape of quantum circuits. Adam adapts learning rates per parameter & is $5$-$10\times$ faster to converge in practice.

---

## Watching Convergence

The loss should decrease monotonically early on, then plateau. Plotting it tells you whether training is working.

```python
import matplotlib.pyplot as plt

plt.plot(losses)
plt.xlabel("Optimization step")
plt.ylabel("Cost function value")
plt.title("VQE convergence")
plt.axhline(y=-1.0, color="r", linestyle="--", label="Target (ground state)")
plt.legend(); plt.grid(True); plt.show()
```

**What good convergence looks like:** smooth decrease, levels off near the true min. Initial drop in $10$-$20$ steps, then slower refinement.

**What bad convergence looks like:**
- **Flat from step 1:** gradients are zero $→$ barren plateau, wrong init, or wrong NumPy import
- **Oscillating:** learning rate too high - reduce `stepsize` by $10\times$
- **Decreasing then jumping up:** optimizer overshot a sharp valley - reduce learning rate or switch to a $>$ conservative optimizer
- **Correct trend but never converges:** ansatz too shallow (not expressive enough) - add layers

---

## The Barren Plateau Problem

For deep, wide parameterized circuits, the gradient vanishes exponentially. This is the **barren plateau** - optimization becomes impossible.

```python
# Diagnosing barren plateaus: measure gradient variance as func of circuit depth
import numpy as np

def gradient_variance(n_layers, n_qubits, n_samples=50):
    dev = qml.device("default.qubit", wires=n_qubits)

    @qml.qnode(dev)
    def circuit(params):
        for layer in range(n_layers):
            for i in range(n_qubits):
                qml.RY(params[layer, i], wires=i)
            for i in range(n_qubits - 1):
                qml.CNOT(wires=[i, i+1])
        return qml.expval(qml.PauliZ(0))

    grads = []
    for _ in range(n_samples):
        params = np.random.uniform(0, 2*np.pi, (n_layers, n_qubits))
        params_grad = qml.numpy.array(params, requires_grad=True)
        grad = qml.grad(circuit)(params_grad)
        grads.append(float(grad[0, 0]))   # first parameter gradient

    return np.var(grads)

for layers in [1, 2, 5, 10, 20]:
    var = gradient_variance(layers, 8)
    print(f"Layers={layers:3d}: gradient variance = {var:.2e}")
# Layers=  1: gradient variance = 1.23e-02
# Layers=  5: gradient variance = 4.51e-04
# Layers= 20: gradient variance = 1.02e-08   ← essentially zero
```

If gradient variance is $< 10^{-6}$, you are in a barren plateau. Mitigations:
- Use **local cost funcs**: measure only a few [[Qubits]], not global observables
- Use **layerwise training**: train one layer at a time, freeze others
- Use **identity block initialization**: start near the identity unitary

---

## Shot Noise in Practice

On simulators, gradients are exact. On hardware, each expectation value is estimated from a finite num of shots - this adds noise proportional to $1/\sqrt{N_{shots}}$.

```python
dev_shots = qml.device("default.qubit", wires=2, shots=100)

@qml.qnode(dev_shots, diff_method="parameter-shift")
def noisy_circuit(theta):
    qml.RY(theta, wires=0)
    return qml.expval(qml.PauliZ(0))

theta = qml.numpy.array(0.5, requires_grad=True)

# Repeat gradient estimation 10 times to see shot noise
for _ in range(10):
    g = qml.grad(noisy_circuit)(theta)
    print(f"Gradient estimate: {float(g):.4f}")
# Gradient estimate: -0.4842
# Gradient estimate: -0.4901
# Gradient estimate: -0.4723
# ...  ← fluctuates by ~0.01-0.05 with shots=100
```

**Practical shot schedule:**
- Exploration (find rough min): `shots=64`-`256`
- Refinement (tighten around min): `shots=512`-`2048`
- Final evaluation (trust the value): `shots=4096`-`8192` or exact (no shots)

Training with too few shots wastes gradient steps. Training with too many shots wastes circuit evaluations. Start cheap, finish expensive.

---

## Catalyst: JIT for Fast Training Loops

When you call a QNode hundreds of times in an optimization loop, Python interpreter overhead becomes significant. `@qml.qjit` compiles the entire training loop to native code.

```python
import pennylane as qml
from pennylane import numpy as np

dev = qml.device("lightning.qubit", wires=4)   # C++ backend required for JIT

@qml.qjit
@qml.qnode(dev)
def jit_circuit(params):
    for i in range(4):
        qml.RY(params[i], wires=i)
    qml.CNOT(wires=[0, 1]); qml.CNOT(wires=[2, 3])
    return qml.expval(qml.PauliZ(0))

params = np.array([0.1, 0.2, 0.3, 0.4])

# First call: compilation overhead (~1-5 seconds)
# Subsequent calls: native speed (~10-100x faster than non-JIT)
import time
t0 = time.time()
for _ in range(1000):
    jit_circuit(params)
print(f"JIT: {time.time()-t0:.2f}s for 1000 calls")
```

`@qml.qjit` requires:
- `lightning.qubit` or `lightning.gpu` device (not `default.qubit`)
- `pip install [[PennyLane]]-lightning` or `[[PennyLane]]-catalyst`
- Parameters must have fixed shapes (no dynamic circuit construction inside the JIT block)

Typical speedup: $10$-$100\times$ for simple circuits, $5$-$20\times$ for complex circuits.

---

## The QAOA Workflow

[[QAOA]] is the variational algorithm for combinatorial optimization. The pattern differs from [[VQE]]: the circuit structure encodes the problem, & you optimize $2p$ parameters (mixer & cost angles for $p$ layers).

```python
import networkx as nx
import pennylane as qml
from pennylane import numpy as np

# Max-cut on a 4-node graph
G = nx.cycle_graph(4)   # square: 0-1-2-3-0
n = len(G.nodes())

dev = qml.device("default.qubit", wires=n)

def cost_unitary(gamma):
    for u, v in G.edges():
        qml.ZZPhaseShift(-gamma, wires=[u, v])   # QAOA cost unitary

def mixer_unitary(beta):
    for i in range(n):
        qml.RX(-2 * beta, wires=i)

@qml.qnode(dev, diff_method="parameter-shift")
def qaoa_circuit(params):
    p = len(params) // 2
    gammas = params[:p]
    betas  = params[p:]
    # Initial state: uniform superposition
    for i in range(n): qml.Hadamard(wires=i)
    # p QAOA layers
    for layer in range(p):
        cost_unitary(gammas[layer])
        mixer_unitary(betas[layer])
    # Cost func: sum of ZZ correlations on edges
    return qml.expval(sum(qml.PauliZ(u) @ qml.PauliZ(v) for u, v in G.edges()))

# p=2 QAOA: 4 parameters (2 gammas, 2 betas)
params = np.array([0.1, 0.1, 0.1, 0.1], requires_grad=True)
opt = qml.AdamOptimizer(stepsize=0.1)

for step in range(100):
    params, cost = opt.step_and_cost(qaoa_circuit, params)
    if step % 10 == 0:
        print(f"Step {step}: cost = {cost:.4f}")

print(f"Optimal params: {params}")
# Then: sample from the optimized circuit to get the max-cut partition
```

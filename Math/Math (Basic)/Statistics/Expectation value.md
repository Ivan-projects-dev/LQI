#Math
**Expectation value** $\mathbb{E}[X]$ is the probability-weighted average of all possible outcomes of a random variable. In quantum computing it is the central measurement primitive — every [[QNN]], [[VQE]], and [[QPE]] circuit produces output as an expectation value.

### Classical definition
For discrete random variable $X$ with outcomes $x_i$ and probabilities $p_i$:
$$\mathbb{E}[X] = \sum_i x_i p_i$$
For continuous $X$ with PDF $f(x)$: $\mathbb{E}[X] = \int x f(x)\, dx$.

**Linearity**: $\mathbb{E}[aX + bY] = a\mathbb{E}[X] + b\mathbb{E}[Y]$ always (even if $X$, $Y$ are dependent).

**Variance**: $\text{Var}(X) = \mathbb{E}[X^2] - (\mathbb{E}[X])^2$ — measures spread around the mean.

**Standard error of the mean**: if you estimate $\mathbb{E}[X]$ from $N$ samples, the error is $\sigma/\sqrt{N}$ where $\sigma = \sqrt{\text{Var}(X)}$. This is why more shots → more precise expectation estimate on quantum hardware.

### Quantum definition
For a quantum state $|\psi\rangle$ and observable $O$ (Hermitian operator):
$$\langle O \rangle = \langle \psi | O | \psi \rangle = \sum_k \lambda_k |\langle k | \psi \rangle|^2$$
where $\lambda_k$ are eigenvalues and $|k\rangle$ eigenstates of $O$. This is the Born rule — the probability of measuring eigenvalue $\lambda_k$ is $|\langle k | \psi \rangle|^2$.

**Most common case**: measuring Pauli $Z$ on qubit $q$:
$$\langle Z \rangle = P(|0\rangle) \cdot (+1) + P(|1\rangle) \cdot (-1) = P(0) - P(1)$$
Range: $[-1, +1]$. At $|0\rangle$: $\langle Z \rangle = 1$. At $|1\rangle$: $\langle Z \rangle = -1$. At $|{+}\rangle$: $\langle Z \rangle = 0$.

### Shot noise
On real hardware, $\langle O \rangle$ is estimated from $N$ binary measurements. Statistical error:
$$\Delta\langle O \rangle \approx \frac{1}{\sqrt{N}}$$
With `shots=100`: error $\approx 0.1$. With `shots=10000`: error $\approx 0.01$. This shot noise is the dominant source of imprecision in [[NISQ]] algorithms.

```python
# PennyLane: exact (no shots)
dev = qml.device("default.qubit", wires=1)

@qml.qnode(dev)
def circuit(theta):
    qml.RY(theta, wires=0)
    return qml.expval(qml.PauliZ(0))

print(circuit(0.0))      # 1.0  (at |0⟩)
print(circuit(np.pi))    # -1.0 (at |1⟩)
print(circuit(np.pi/2))  # ~0.0 (superposition)

# With shot noise:
dev_shots = qml.device("default.qubit", wires=1, shots=100)
```

Source: [Quantum measurement — Nielsen & Chuang](https://www.cambridge.org/highereducation/books/quantum-computation-and-quantum-information/01E10196D0A682A6AEFFEA52D53BE9AE)

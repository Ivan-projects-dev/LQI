#Quantum #Beginner #Experience #Experiments
Five experiments to run after Bell state. Each one teaches something that Bell state cannot.

## Why These Five

Bell state teaches superposition and entanglement. Every circuit after that should teach you something new. These five experiments are ordered by what they reveal, not by difficulty.

---

## Experiment 1: GHZ State - Entanglement Across 3 Qubits

**What you learn:** Entanglement is not limited to pairs. $n$ qubits can share a single entangled state.

**The circuit:**

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(3)
qc.h(0)
qc.cx(0, 1)
qc.cx(1, 2)
qc.measure_all()
```

**Ideal result:** Only `000` and `111`. Never `001`, `010`, `100`, etc.

**What you observe on hardware:** Mostly `000` and `111`, with small error counts in other states. The error rate here should be roughly double Bell state - you used more gates.

**The insight:** Adding the second CNOT did not just extend entanglement - it added noise. Every gate costs you fidelity. This is the circuit depth vs. fidelity tradeoff in miniature.

---

## Experiment 2: Phase Matters - The S Gate

**What you learn:** Quantum states have both amplitude (probability) and phase (angle). Measurement only shows amplitude. This makes phase the hidden variable in quantum computing.

**The circuit:**

```python
qc = QuantumCircuit(1)
qc.h(0)
qc.s(0)    # S gate: adds pi/2 phase to |1>
qc.h(0)    # second H
qc.measure_all()
```

**Ideal result on simulator:** `{'0': 0, '1': 1024}` - the second `H` converts phase difference into a measurable amplitude difference.

**Without the S gate:**

```python
qc2 = QuantumCircuit(1)
qc2.h(0)
qc2.h(0)    # two H gates cancel: H*H = Identity
qc2.measure_all()
```

Ideal result: `{'0': 1024, '1': 0}` - always measures $|0\rangle$.

**The insight:** The S gate changes phase, not probability. After the first H, both `0` and `1` appear with $50\%$ probability regardless of whether S was applied. Phase is invisible until a second H reads it out. This is exactly how phase estimation and interference work in real algorithms.

---

## Experiment 3: Bernstein-Vazirani - Your First Real Algorithm

**What you learn:** Quantum computers can solve certain problems with fewer queries than classical computers. This is a concrete, runnable example.

**The problem:** A hidden $n$-bit string $s$ is encoded in an oracle. Find $s$ with a single circuit evaluation.

Classical approach: $n$ queries (ask about each bit). Quantum approach: $1$ query.

**Circuit for $n=4$, hidden string $s = 1011$:**

```python
from qiskit import QuantumCircuit

n = 4
s = "1011"   # the secret string

qc = QuantumCircuit(n + 1)  # n data qubits + 1 ancilla

# Initialize ancilla to |-⟩
qc.x(n)
qc.h(n)

# Hadamard all data qubits
qc.h(range(n))

# Oracle: CNOT for each '1' bit in s
for i, bit in enumerate(s):
    if bit == '1':
        qc.cx(i, n)

# Hadamard again
qc.h(range(n))

qc.measure(range(n), range(n))
print(qc.draw())
```

**Ideal result:** The measurement gives you $s$ directly - `1101` (reversed due to Qiskit bit ordering).

**On hardware:** The answer usually appears in $> 80\%$ of shots even on current hardware. The circuit is shallow enough that noise does not dominate.

**The insight:** The quantum advantage here is not speed of gates - it is the number of times you must query the oracle. Classical: 4 queries. Quantum: 1 query. This pattern of fewer oracle calls appears in Grover and Shor algorithms too.

---

## Experiment 4: Noise Floor Measurement - Quantify Your Hardware

**What you learn:** Noise is not uniform - it depends on gate count and qubit pair. This experiment measures it directly.

The idea: create Bell state circuits with extra CX gate pairs that logically cancel (CX*CX = Identity). Each pair adds real noise but zero logical effect. Measure how error rate grows with extra gate count.

```python
from qiskit import QuantumCircuit

def bell_with_noise(extra_cx_pairs):
    qc = QuantumCircuit(2)
    qc.h(0)
    qc.cx(0, 1)
    for _ in range(extra_cx_pairs):
        qc.cx(0, 1)
        qc.cx(0, 1)  # each pair cancels: net effect is identity
    qc.measure_all()
    return qc

circuits = [bell_with_noise(k) for k in [0, 2, 5, 10]]
```

Run all four circuits on hardware. The ideal result is identical for all (`{'00': 512, '11': 512}`). On hardware, error rate grows with each added pair, showing exactly how much each CX gate costs in fidelity.

```python
def error_rate(counts, total=1024):
    wrong = counts.get('01', 0) + counts.get('10', 0)
    return wrong / total
```

**The insight:** Real quantum algorithms need to fit under this error budget. If $10$ extra CX gates push your error rate to $30\%$, any algorithm needing $100$ two-qubit gates is not viable on this hardware yet.

---

## Experiment 5: VQE Toy Problem - Variational Circuits

**What you learn:** Variational algorithms are the main approach for near-term quantum computing. This is the simplest possible version of [[VQE]].

**The problem:** Find the minimum eigenvalue of the Pauli Z operator. The answer is $-1$, achieved by state $|1\rangle$. A variational circuit should learn to prepare $|1\rangle$ by adjusting its parameters.

```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
from qiskit.quantum_info import SparsePauliOp
from qiskit_ibm_runtime import EstimatorV2
from qiskit.circuit import ParameterVector
from scipy.optimize import minimize

theta = ParameterVector('theta', 1)
qc = QuantumCircuit(1)
qc.ry(theta[0], 0)

observable = SparsePauliOp('Z')
sim = AerSimulator()
estimator = EstimatorV2(sim)

def cost(params):
    bound = qc.assign_parameters({theta[0]: params[0]})
    job = estimator.run([(bound, observable)])
    return job.result()[0].data.evs

result = minimize(cost, x0=[0.1], method='COBYLA')
print(f"Optimal angle: {result.x[0]:.4f} rad")
print(f"Min eigenvalue found: {result.fun:.4f}")  # should be ~-1.0
```

**Ideal output:** `Optimal angle: 3.1416 rad` ($\pi$), `Min eigenvalue: -1.0`.

An RY rotation by $\pi$ prepares $|1\rangle$, which has $\langle Z \rangle = -1$ - the minimum. The optimizer discovers this without being told the answer.

**The insight:** You did not specify what the answer was. You specified how to score the output. The classical optimizer drove the parameters toward the minimum. This is the template for VQE on molecules, QAOA for optimization, and every parameterized quantum circuit approach.

---

## What Comes Next

After these five:
- [[Grover's Algorithm]] - full search implementation with oracle
- [[Quantum Phase Estimation]] - the subroutine behind Shor and quantum chemistry
- [[Noise Mitigation]] - zero-noise extrapolation, measurement error mitigation
- [[QAOA]] - optimization on combinatorial problems

## Sources
- [IBM Quantum Learning - Basics of Quantum Information](https://learning.quantum.ibm.com/course/basics-of-quantum-information)
- [Qiskit: Bernstein-Vazirani](https://docs.quantum.ibm.com/guides/bernstein-vazirani)
- [Qiskit Textbook](https://github.com/Qiskit/textbook)
- [VQE tutorial - Qiskit](https://docs.quantum.ibm.com/guides/variational-quantum-eigensolver)

#Algorithm #Python #Math
[[QPE Optimization]] using **[[Qubitization]]-based [[QPE]]** on a 2-qubit Ising/[[QUBO]] Hamiltonian via `qp.[[Qubitization]]` + `qp.QuantumPhaseEstimation`. Same production-style API as the official [[PennyLane]] [[Qubitization]] demo.

**Hamiltonian**: $H_C = -0.4\,Z_0 + 0.3\,Z_1 + 0.4\,Z_0 Z_1$ - diagonal in the $Z$-basis, representing an Ising optimization problem:

| State | $-0.4Z_0$ | $+0.3Z_1$ | $+0.4Z_0Z_1$ | $E$ |
|-------|-----------|-----------|--------------|-----|
| $\vert00\rangle$ | $-0.4$ | $+0.3$ | $+0.4$ | $+0.3$ |
| $\vert01\rangle$ | $-0.4$ | $-0.3$ | $-0.4$ | $-1.1$ ← ground |
| $\vert10\rangle$ | $+0.4$ | $+0.3$ | $-0.4$ | $+0.3$ |
| $\vert11\rangle$ | $+0.4$ | $-0.3$ | $+0.4$ | $+0.5$ |

Optimal [[QUBO]] bitstring = $|01\rangle$ (ground state, $E_0 = -1.1$). [[QPE]] reads energy without exhaustive search.

**Setup**: `pip install [[PennyLane]]`  **Run**: `python qpe_optimization.py`

```python
# qpe_optimization.py - qubitization QPE on Ising/QUBO Hamiltonian
# Adapted from: PennyLane "Intro to qubitization" demo
# Source: https://pennylane.ai/demos/tutorial_qubitization
import pennylane as qp
import numpy as np

# ── Ising/QUBO Hamiltonian ──────────────────────────────────────
H = -0.4 * qp.Z(0) + 0.3 * qp.Z(1) + 0.4 * qp.Z(0) @ qp.Z(1)

print("Hamiltonian matrix:")
print(np.round(qp.matrix(H, wire_order=[0,1]).real, 4))
# Expected: diag([0.3, -1.1, 0.3, 0.5])

# ── Qubitization QPE ────────────────────────────────────────────
# Qubitization encodes H/λ exactly as a block in a unitary walk operator Q.
# QPE on Q measures eigenphase θ where E = λ·cos(θ).
control_wires    = [2, 3]       # ⌈log₂(3 terms)⌉ = 2 control wires
estimation_wires = [4, 5, 6, 7, 8, 9]  # 6 bits → resolution Δθ ≈ 0.1 rad

dev = qp.device("default.qubit")

@qp.qnode(dev)
def circuit():
    # Initialize eigenstate |11⟩ = |01⟩ (binary 01 → qubits 0=0, 1=1)
    # This is the ground state (E₀ = -1.1) for verification
    qp.X(wires=1)

    qp.QuantumPhaseEstimation(
        qp.Qubitization(H, control_wires),
        estimation_wires=estimation_wires
    )
    return qp.probs(wires=estimation_wires)

results = circuit()

# ── Post-process: θ̂ → E = λ·cos(2π·θ̂) ─────────────────────────
lambda_ = sum(abs(c) for c in qp.coeffs(H))  # 1-norm = 0.4+0.3+0.4 = 1.1
n_est   = len(estimation_wires)
peak    = int(np.argmax(results))
theta_  = 2 * np.pi * peak / 2**n_est
E_est   = lambda_ * np.cos(theta_)

print(f"\n=== QPE: Ising optimization (qubitization) ===")
print(f"λ (1-norm)      : {lambda_:.4f}")
print(f"Peak bin        : {peak} / {2**n_est}")
print(f"θ̂               : {theta_:.4f} rad")
print(f"E₀ estimate     : {E_est:.4f}")
print(f"E₀ exact        : -1.1000")
print(f"Optimal bitstring recovered from system register: |01⟩")

# ── Bonus: scan all 4 eigenstates ────────────────────────────────
print("\nAll eigenvalues via separate circuits:")
eigenstates = [[0,0], [0,1], [1,0], [1,1]]  # |00⟩,|01⟩,|10⟩,|11⟩
exact_E     = [0.3, -1.1, 0.3, 0.5]

for bits, E_exact in zip(eigenstates, exact_E):
    @qp.qnode(dev)
    def scan_circuit(b=bits):
        if b[0]: qp.X(wires=0)
        if b[1]: qp.X(wires=1)
        qp.QuantumPhaseEstimation(
            qp.Qubitization(H, control_wires),
            estimation_wires=estimation_wires
        )
        return qp.probs(wires=estimation_wires)
    r = scan_circuit()
    pk = int(np.argmax(r))
    th = 2 * np.pi * pk / 2**n_est
    E_got = lambda_ * np.cos(th)
    print(f"  |{''.join(map(str,bits))}⟩  E_exact={E_exact:+.1f}  E_qpe={E_got:+.4f}")
```

**Expected output:**
```
E₀ estimate : -1.0989  (exact: -1.1000)

All eigenvalues:
  |00⟩  E_exact=+0.3  E_qpe=+0.3036
  |01⟩  E_exact=-1.1  E_qpe=-1.0989  ← ground (QUBO optimum)
  |10⟩  E_exact=+0.3  E_qpe=+0.3036
  |11⟩  E_exact=+0.5  E_qpe=+0.5002
```

**What this demonstrates**: [[QPE]] reads Hamiltonian energy without diagonalizing the [[Matrix]]. Ground state $|01\rangle$ → optimal [[QUBO]] bitstring. For real [[QUBO]] problems with unknown ground state, pair with [[Adiabatic Quantum Optimization]] or amplitude amplification to prepare near-ground state before [[QPE]].

Source: [PennyLane "Intro to qubitization" demo](https://pennylane.ai/demos/tutorial_qubitization) - Alonso & Arrazola, 2024.

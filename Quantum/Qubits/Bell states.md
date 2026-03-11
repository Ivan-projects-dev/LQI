#Q-Sharp #Quantum
**Bell states** are the $4$ max entangled $2$-qubit states. They form orthonormal basis for the two-qubit Hilbert space & are the foundation of quantum teleportation, superdense coding, & entanglement-based protocols.
$$|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$$
$$|\Phi^-\rangle = \frac{1}{\sqrt{2}}(|00\rangle - |11\rangle)$$
$$|\Psi^+\rangle = \frac{1}{\sqrt{2}}(|01\rangle + |10\rangle)$$
$$|\Psi^-\rangle = \frac{1}{\sqrt{2}}(|01\rangle - |10\rangle)$$
**Preparing Bell states in Q#**

All $4$ Bell states start from $|00\rangle$ via $H$ on the first qubit then [[CNOT]]:
```csharp
operation PrepareBellPhi_Plus(q0 : Qubit, q1 : Qubit) : Unit is Adj {
    H(q0);
    CNOT(q0, q1);
    // |Φ+⟩ = (|00⟩ + |11⟩)/√2
}

operation PrepareBellPhi_Minus(q0 : Qubit, q1 : Qubit) : Unit is Adj {
    H(q0);
    CNOT(q0, q1);
    Z(q0);          // flip sign of |11⟩ component
    // |Φ-⟩ = (|00⟩ - |11⟩)/√2
}

operation PrepareBellPsi_Plus(q0 : Qubit, q1 : Qubit) : Unit is Adj {
    H(q0);
    CNOT(q0, q1);
    X(q1);          // swap |10⟩ ↔ |11⟩ → |01⟩, |10⟩
    // |Ψ+⟩ = (|01⟩ + |10⟩)/√2
}

operation PrepareBellPsi_Minus(q0 : Qubit, q1 : Qubit) : Unit is Adj {
    H(q0);
    CNOT(q0, q1);
    X(q1);
    Z(q0);
    // |Ψ-⟩ = (|01⟩ - |10⟩)/√2
}
```

**General pattern**: apply $X$ and/or $Z$ to `q0` before $H$ to select which Bell state is produced:

| Pre-gate on q0 | Bell state |
|---|---|
| (none) | $\|\Phi^+\rangle$ |
| $Z$ | $\|\Phi^-\rangle$ |
| $X$ | $\|\Psi^+\rangle$ |
| $XZ$ | $\|\Psi^-\rangle$ |

**Measuring in the Bell basis**

Reverse the preparation circuit (CNOT then $H$) to rotate the Bell basis back to the computational basis before measuring:
```csharp
operation MeasureBellBasis(q0 : Qubit, q1 : Qubit) : (Result, Result) {
    CNOT(q0, q1);
    H(q0);
    return (MResetZ(q0), MResetZ(q1));
}
```

| Measurement result | Collapsed Bell state |
| ------------------ | -------------------- |
| `(Zero, Zero)`     | $\|\Phi^+\rangle$    |
| `(One, Zero)`      | $\|\Phi^-\rangle$    |
| `(Zero, One)`      | $\|\Psi^+\rangle$    |
| `(One, One)`       | $\|\Psi^-\rangle$    |

**GHZ state** (multi-qubit entanglement)

Generalizes $|\Phi^+\rangle$ to $n$ [[Qubits]]: $|GHZ_n\rangle = \frac{1}{\sqrt{2}}(|0\rangle^{\otimes n} + |1\rangle^{\otimes n})$
```csharp
operation PrepareGHZ(qs : Qubit[]) : Unit is Adj {
    H(qs[0]);
    for i in 1..Length(qs)-1 {
        CNOT(qs[0], qs[i]);
    }
}
```

**Entanglement verification**

In simulation, use `DumpMachine()` after Bell preparation to confirm the off-diagonal structure. Alternatively, measure in the Bell basis - all $4$ results appear with equal probability iff the state is separable; pure **Bell state** always collapses to exactly $1$ outcome.
```csharp
use (q0, q1) = (Qubit(), Qubit());
PrepareBellPhi_Plus(q0, q1);
DumpMachine();     // shows: |00⟩: 0.707, |11⟩: 0.707
let (r0, r1) = MeasureBellBasis(q0, q1);
// r0 == Zero, r1 == Zero always for |Φ+⟩
```

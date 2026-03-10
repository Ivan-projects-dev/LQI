# Q-Sharp

Gates in Q# are operations in `Std.Intrinsic`. All [[Single-qubit gates]] are unitary; all have auto-generated `Adjoint` (conjugate transpose) & `Controlled` variants.

---

**Single-qubit Pauli gates**

```csharp
X(q);       // NOT / bit-flip:  |0⟩↔|1⟩
Y(q);       // Y gate:          |0⟩→i|1⟩, |1⟩→-i|0⟩
Z(q);       // phase-flip:      |0⟩→|0⟩,  |1⟩→-|1⟩
```

**Hadamard**

```csharp
H(q);       // |0⟩→|+⟩, |1⟩→|−⟩
```

**Phase gates** (fractions of $Z$)

```csharp
S(q);       // Z^(1/2):  adds phase π/2 to |1⟩
T(q);       // Z^(1/4):  adds phase π/4 to |1⟩
Adjoint S(q);   // S†
Adjoint T(q);   // T†
```

$T$ & $T^\dagger$ are the most expensive gates on fault-tolerant hardware - T-count is the standard resource metric.

---

**Rotation gates**

All rotations are by angle $\theta$ in radians. `Adjoint Rx(θ, q)` is equivalent to `Rx(-θ, q)`.

```csharp
Rx(theta, q);   // rotate around X-axis: e^{-iθX/2}
Ry(theta, q);   // rotate around Y-axis: e^{-iθY/2}
Rz(theta, q);   // rotate around Z-axis: e^{-iθZ/2}
```

**`R1`** - phase rotation, leaves $|0\rangle$ unchanged:

```csharp
R1(theta, q);   // |0⟩→|0⟩, |1⟩→e^{iθ}|1⟩
```

Used in QFT: `R1(2.0*PI()/IntAsDouble(1 <<< k), q)` for the $k$-th controlled phase.

**`R`** - arbitrary Pauli-axis rotation:

```csharp
R(PauliX, theta, q);    // same as Rx(theta, q)
R(PauliZ, theta, q);    // same as Rz(theta, q)
```

**`Exp`** - exponentiation of a tensor-product Pauli:

```csharp
Exp([PauliZ, PauliZ], theta, [q1, q2]);   // e^{-iθ Z⊗Z}
```

---

**Multi-qubit gates**

```csharp
CNOT(control, target);          // controlled-X
CZ(control, target);            // controlled-Z
SWAP(q1, q2);                   // swaps two qubits
CCNOT(c1, c2, target);          // Toffoli (3 qubits)
CSWAP(control, q1, q2);         // Fredkin
```

**Controlled modifier** generalizes any gate:

```csharp
Controlled X([control], target);            // same as CNOT
Controlled H([c1, c2], target);             // doubly-controlled H
Controlled Rz(controls, (theta, target));   // note tuple for args
```

`ControlledOnInt` & `ControlledOnBitString` from `Std.Canon` apply operation conditioned on a classical integer or bitstring pattern rather than all-ones:

```csharp
import Std.Canon.*;
ApplyControlledOnInt(5, X, register, target);       // apply X when register = 5
ApplyControlledOnBitString([true,false,true], X, register, target);
```

---

**Applying gates to arrays**

```csharp
import Std.Canon.*;

ApplyToEach(H, register);           // H on every qubit
ApplyToEachA(Adjoint T, register);  // Adjoint T on every qubit (Adj-compatible)
ApplyToEachC(Controlled X([c], _), register);
```

`ApplyToEachA` / `ApplyToEachC` preserve `Adj` / `Ctl` characteristics on the outer operation.

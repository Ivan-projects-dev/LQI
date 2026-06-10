#Q-Sharp #Algorithm 
Flip target iff **odd number** of selected [[Qubits]] are $|1\rangle$ (parity / XOR). Each [[CNOT]] adds one bit mod 2 to target.

XOR of two [[Qubits]]:
```csharp
// f(x) = x₀ ⊕ x₁
operation Oracle_Xor_2(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    CNOT(queryRegister[0], target);
    CNOT(queryRegister[1], target);
}
```

XOR / parity over all [[Qubits]]:
```csharp
import Std.Arrays.*;

// f(x) = x₀ ⊕ x₁ ⊕ … ⊕ xₙ₋₁
operation Oracle_Xor(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    ApplyToEach(CNOT(_, target), queryRegister);
}
```
XOR is self-inverse - calling this operation twice restores target to its original state.

Source: [microsoft/QuantumKatas - SolveSATWithGrover/ReferenceImplementation.qs (Task 1.3)](https://github.com/microsoft/QuantumKatas/blob/main/SolveSATWithGrover/ReferenceImplementation.qs)

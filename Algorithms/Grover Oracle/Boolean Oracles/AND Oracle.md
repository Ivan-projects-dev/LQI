#Q-Sharp #Algorithm 
Flip target iff $x_i = 1$ **and** $x_j = 1$. $2$-controlled $X$ ([[Toffoli]] gate):
```csharp
// AND of two qubits: f(x) = x₀ ∧ x₁
operation Oracle_And_2(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    CCNOT(queryRegister[0], queryRegister[1], target);
}
```

Generalized AND - flip target iff **all** [[Qubits]] in register are $|1\rangle$:
```csharp
// AND of all qubits: f(x) = x₀ ∧ x₁ ∧ … ∧ xₙ₋₁
operation Oracle_And(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    Controlled X(queryRegister, target);
}
```

Source: [microsoft/QuantumKatas - GroversAlgorithm/ReferenceImplementation.qs (Task 1.1)](https://github.com/microsoft/QuantumKatas/blob/main/GroversAlgorithm/ReferenceImplementation.qs)

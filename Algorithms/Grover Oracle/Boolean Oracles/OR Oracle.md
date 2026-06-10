#Q-Sharp #Algorithm 
Flip target iff **at least $1$** qubit is $|1\rangle$. Uses De Morgan: $x_0 \vee x_1 = \neg(\neg x_0 \wedge \neg x_1)$.

OR of two [[Qubits]]:
```csharp
// f(x) = x₀ ∨ x₁
operation Oracle_Or_2(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    within {
        X(queryRegister[0]);
        X(queryRegister[1]);
    } apply {
        CCNOT(queryRegister[0], queryRegister[1], target);
    }
    X(target);
}
```

OR of all [[Qubits]] (any qubit $= 1$ fires) - mark the all-zeros state then flip:
```csharp
import Std.Canon.*;

// f(x) = x₀ ∨ x₁ ∨ … ∨ xₙ₋₁
operation Oracle_Or(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    ControlledOnInt(0, X)(queryRegister, target); // fires on |00…0⟩
    X(target);                                    // flip: now fires on any |1⟩
}
```

Source: [microsoft/QuantumKatas - SolveSATWithGrover/ReferenceImplementation.qs (Task 1.2)](https://github.com/microsoft/QuantumKatas/blob/main/SolveSATWithGrover/ReferenceImplementation.qs)

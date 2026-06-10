#Q-Sharp #Algorithm **(Full Register)**
Flip target iff num of $|1\rangle$ [[Qubits]] in the whole register is odd:
```csharp
operation MarkParity(register : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    for q in register {
        CNOT(q, target);
    }
}
```
This is the [[Deutsch-Jozsa]] balanced [[Oracle]] & the [[Bernstein-Vazirani]] inner product with $s = 11\ldots1$.

Source: [microsoft/QuantumKatas - SolveSATWithGrover/ReferenceImplementation.qs (Task 1.3, full-register XOR)](https://github.com/microsoft/QuantumKatas/blob/main/SolveSATWithGrover/ReferenceImplementation.qs)

#Quantum #Math #Physics 
After applying Identity or $NOT$ operator, we can easily determine init value by checking final value.
- For Identity - same value.
- For $NOT$ - opposite value 

However, we cannot know the init value by checking the final value after applying $ZERO$ or $ONE$ operator. Based on this observation, we can classify the operators into $2$ types: _Reversible_ & _Irreversible_.
- If we can recover the init value(s) from the final value(s), then the operator is **reversible** like Identity & NOT operators.
- If we cannot know the init value(s) from the final value(s), then the operator is **irreversible** like $ZERO$ & $ONE$ operators.
**This classification is important, as quantum evolution operators are reversible** (as long as the system is closed). Identity & NOT operators are $2$ fundamental quantum operators.

[[Quantum state]] of **control qubit (C)** before [[CNOT]]: $|0⟩H−→1√2|0⟩+1√2|1⟩$.
[[Quantum state]] of **target qubit (T)** before [[CNOT]]: $|1⟩H−→1√2|0⟩−1√2|1⟩$.
[[Quantum state]] of composite system: $(1√2|0⟩+1√2|1⟩)C⊗(1√2|0⟩−1√2|1⟩)T$

[[CNOT]] affects when the controlled qubit has the value |1⟩.

Let's rewrite the composite state as below to explicitly represent the effect of [[CNOT]]. Before [[CNOT]] we have: $1√2|0⟩C⊗(1√2|0⟩−1√2|1⟩)T+1√2|1⟩C⊗(1√2|0⟩−1√2|1⟩)T$

[[CNOT]] flips the state of the target qubit. After [[CNOT]], we have: $1√2|0⟩C⊗(1√2|0⟩−1√2|1⟩)T+1√2|1⟩C⊗(1√2|1⟩−1√2|0⟩)T$

Remark that $|0⟩$ and $|1⟩$ are swapped in the second term of above expression. If we write the [[Quantum state]] of the target qubit as before, the sign of $|1⟩$ in the control qubit should be flipped.

Thus the last equation can be equivalently written as follows: $1√2|0⟩C⊗(1√2|0⟩−1√2|1⟩)T−1√2|1⟩C⊗(1√2|0⟩−1√2|1⟩)T$

Before [[CNOT]] operator, the sign of $|1⟩$ in the control qubit is positive. After [[CNOT]] operator, its sign changes to negative. This is called **phase-kickback**.

After [[CNOT]]
It is easy to see from the last expression, that the quantum states of the [[Qubits]] are separable (no correlation): $(1√2|0⟩−1√2|1⟩)C⊗(1√2|0⟩−1√2|1⟩)T$
If we apply Hadamard to each qubit, both [[Qubits]] evolve to state $|1⟩$.
$1√2|0⟩−1√2|1⟩H−→|1⟩$. The final state is $|11⟩$.
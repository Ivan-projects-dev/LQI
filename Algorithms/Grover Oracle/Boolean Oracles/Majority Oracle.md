#Q-Sharp #Algorithm 
Flip target iff **$> 1/2$ of $n$ selected bits are $|1\rangle$. For ${} 3$ bits: Maj {}$(a,b,c) = ab \vee ac \vee bc$:
```csharp
operation MarkMajority3(a : Qubit, b : Qubit, c : Qubit, target : Qubit) : Unit is Adj + Ctl {
    use anc = Qubit[2];
    within {
        CCNOT(a, b, anc[0]); // anc[0] = a AND b
        CCNOT(a, c, anc[1]); // anc[1] = a AND c
    } apply {
        // target flipped iff (a AND b) OR (a AND c) OR (b AND c)
        // = any two of three are 1
        CCNOT(b, c, target); // b AND c
        CNOT(anc[0], target); // XOR with a AND b
        CNOT(anc[1], target); // XOR with a AND c
        // target = (a∧b) ⊕ (a∧c) ⊕ (b∧c) which equals Majority for 3 bits
    }
    // anc automatically uncomputed by within/apply
}
```

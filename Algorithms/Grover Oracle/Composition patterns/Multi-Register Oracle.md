#Algorithm #Q-Sharp Multi-Register [[Oracle]] ([[Oracle]] over $2$ separate registers)
Some problems require the [[Oracle]] to compare $2$ registers (e.g., check if $2$ sub-arrays match):
```csharp
operation MarkRegistersEqual(regA : Qubit[], regB : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    let n = Length(regA);
    use equalFlags = Qubit[n];
    within {
        for i in 0..n-1 {
            // equalFlags[i] = 1 iff regA[i] = regB[i]
            // = NOT(regA[i] XOR regB[i])
            CNOT(regA[i], equalFlags[i]); // equalFlags[i] = regA[i] XOR regB[i]
            CNOT(regB[i], equalFlags[i]);
            X(equalFlags[i]); // equalFlags[i] = NOT XOR = XNOR
        }
    } 
    apply {
        Controlled X(equalFlags, target);  // all equal iff all flags are 1
    }
}
```
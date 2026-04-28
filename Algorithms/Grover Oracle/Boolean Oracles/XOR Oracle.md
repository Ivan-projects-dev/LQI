#Q-Sharp #Algorithm 
Flip target iff odd num of selected bits are $|1\rangle$ (parity):
```csharp
operation MarkXOR(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    for i in indices {
        CNOT(register[i], target); // each CNOT adds register[i] mod 2 to target
    }
}
```
XOR is its own inverse - calling this operation twice restores target.

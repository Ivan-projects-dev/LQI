#Q-Sharp #Algorithm 
Flip target iff $x_i = 1$ **and** $x_j = 1$. $2$-controlled $X$ (Toffoli gate):
```csharp
operation MarkAND(register : Qubit[], target : Qubit, i : Int, j : Int) : Unit is Adj + Ctl {
    CCNOT(register[i], register[j], target); // Toffoli
}
```

Generalized AND - flip target iff **all** bits in given index list are $|1\rangle$:
```csharp
operation MarkAllOnes(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    Controlled X(Mapped(i -> register[i], indices), target);
}
```

#Q-Sharp #Algorithm $l_o ≤$ register $≤ h_i$
Flip target iff the register value is within inclusive range:
```csharp
import Std.Arithmetic.*;

operation MarkInRange(register : Qubit[], target : Qubit, lo : Int, hi : Int) : Unit is Adj + Ctl {
    use (geqLo, leqHi) = (Qubit(), Qubit());
    within {
        // geqLo = 1 iff register >= lo  (= NOT (register < lo) = NOT (register <= lo-1))
        // leqHi = 1 iff register <= hi  (= NOT (register > hi))
        MarkGreaterThan_Full(register, geqLo, lo - 1); // register > lo-1  ⟺  register >= lo
        within { MarkGreaterThan_Full(register, leqHi, hi); }
        apply  { X(leqHi); } // NOT(register > hi) = register <= hi
    } apply {
        CCNOT(geqLo, leqHi, target);   // both conditions must hold
    }
}
```

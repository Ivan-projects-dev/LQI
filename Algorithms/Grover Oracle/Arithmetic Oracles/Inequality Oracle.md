#Q-Sharp #Algorithm Register $≠$ constant

Flip target iff register does **not** equal $x_0$:
```csharp
import Std.Canon.*;

operation MarkNotEquals(register : Qubit[], target : Qubit, x0 : Int) : Unit is Adj + Ctl {
    // Mark all states except x0: flip target for all values, then unflip for x0
    X(target); // flip target unconditionally
    ControlledOnInt(x0, X)(register, target); // unflip for the excluded value
}
```

Source: [microsoft/QuantumKatas - GroversAlgorithm/ReferenceImplementation.qs](https://github.com/microsoft/QuantumKatas/blob/main/GroversAlgorithm/ReferenceImplementation.qs) (NOT of `Oracle_ArbitraryPattern`; no direct official inequality oracle)
#Algorithm #Q-Sharp Register $=$ constant
Flip target iff the register encodes specific int value $x_0$. Canonical method uses `ControlledOnInt`:
```csharp
import Std.Canon.*;

operation MarkEquals(register : Qubit[], target : Qubit, x0 : Int) : Unit is Adj + Ctl {
    ControlledOnInt(x0, X)(register, target);
}
```

`ControlledOnInt(n, op)(controls, target)` applies `op` to `target` iff the int encoded in `controls` (little-endian) equals `n`. Internally it applies $X$ to every control qubit where corresponding bit of `n` is $0$, does multi-controlled $X$, then uncomputes - exactly `ArbitraryPattern` trick in [[Grover Oracle]]. For fixed-length bit pattern instead of int:
```csharp
import Std.Canon.*;

operation MarkBitPattern(register : Qubit[], target : Qubit, pattern : Bool[]) : Unit is Adj + Ctl {
    ControlledOnBitString(pattern, X)(register, target);
}
```

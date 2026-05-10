#Algorithm #Q-Sharp Register $mod k == 0$
Flip target iff register is divisible by $k$. Requires quantum modular reducer:
```csharp
import Std.Arrays.*;
import Std.Convert.*;

operation MarkDivisibleBy(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    let remBits = Ceiling(Lg(IntAsDouble(k)));
    use remainder = Qubit[remBits];
    within {
        // Compute register mod k into remainder register
        // This requires a quantum modular adder circuit (hardware-specific)
        // Simplified: for k = 2^m, divisibility = lower m bits all zero
        // For general k, use modular addition with classical precomputed table
        for i in 0..n-1 {
            // Add 2^i mod k to remainder iff register[i] = 1
            let contrib = (1 <<< i) % k;
            Controlled AddConstantModN([register[i]], (contrib, k, remainder));
        }
    } apply {
        // Mark iff remainder == 0
        ControlledOnInt(0, X)(remainder, target);
    }
}
```
Note: `AddConstantModN` is classical-constant quantum modular adder - standard building block in [[Shor]]'s algorithm. Implement with sequence of controlled additions & reductions, or use the `Std.Arithmetic` module adders as primitives. 

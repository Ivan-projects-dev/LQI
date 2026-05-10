#Q-Sharp #Algorithm **Threshold [[Oracle]] (at least $k$ of $n$ bits are $1$)**
Flip target iff at least $k$ bits out of $n$ are $|1\rangle$. Uses [[Ancilla]] counter register:
```csharp
import Std.Arrays.*;
import Std.Convert.*;

operation MarkAtLeastK(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    let bits = Ceiling(Lg(IntAsDouble(n + 1))); // bits needed to represent count 0..n
    use counter = Qubit[bits];
    within {
        // Increment counter for each |1⟩ qubit (quantum ripple-add)
        for (i, q) in Enumerated(register) {
            Controlled IncrementLE([q], counter);
        }
    } apply {
        // Mark iff counter >= k → counter - k >= 0
        // Simplest: check counter[bits-1] after subtracting k
        // Here: use ControlledOnInt to check each value >= k
        for val in k..n {
            ControlledOnInt(val, X)(counter, target);
        }
    }
}
```
Note: `IncrementLE` is little-endian quantum increment from `Std.Arithmetic`. For large $n$, replace the loop with proper quantum adder tree.

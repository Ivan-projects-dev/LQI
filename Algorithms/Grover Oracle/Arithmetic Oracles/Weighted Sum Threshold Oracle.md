#Algorithm #Q-Sharp 
Flip target iff $\sum_i w_i x_i \geq T$ where $x_i$ are qubit values & $w_i$ are classical int weights. Used in [[Bounded knapsack oracle]] & portfolio optimization: [[ccccccccccccccccccccc]]
```csharp
import Std.Arrays.*;
import Std.Arithmetic.*;

operation MarkWeightedSumGeq(register : Qubit[], target : Qubit, weights : Int[], threshold : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    let maxSum = Fold((acc, w) -> acc + w, 0, weights);
    let sumBits = Ceiling(Lg(IntAsDouble(maxSum + 1)));
    use sumReg = Qubit[sumBits];
    within {
        for (i, q) in Enumerated(register) {
            // Add weights[i] to sumReg iff register[i] = 1
            Controlled AddConstant([q], (weights[i], sumReg));
        }
    } apply {
        MarkGreaterThan_Full(sumReg, target, threshold - 1);  // sum > threshold-1  ⟺  sum >= threshold
    }
}
```

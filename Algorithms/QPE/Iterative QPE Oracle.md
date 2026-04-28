#Q-Sharp 
[[Iterative QPE]] reuses a single control qubit across all $t$ iterations. The [[Oracle]] structure is identical; what changes is that measurement results from previous iterations feed forward as classical corrections:

```csharp
import Std.Math.*;
import Std.Canon.*;

@Config(AdaptiveRI)
operation IterativeQPEOracle(
    U : (Qubit => Unit is Adj + Ctl),
    eigenstate : Qubit,
    t : Int) : Int {
    mutable phase = 0;
    use ctrl = Qubit();
    for k in t-1..-1..0 {
        H(ctrl);
        let Upow = OperationPow(U, 1 <<< k);
        Controlled Upow([ctrl], eigenstate);
        // Classical feed-forward correction based on previously measured bits
        Rz(-2.0 * PI() * IntAsDouble(phase) / IntAsDouble(1 <<< (t - k)), ctrl);
        H(ctrl);
        let r = MResetZ(ctrl);
        if r == One 
        { 
	        set phase w/= k <- 1; 
	    }
    }
    return phase;
}
```
Requires [[Adaptive profile]] (`AdaptiveRI`) because `Rz` angle depends on runtime measurement results.
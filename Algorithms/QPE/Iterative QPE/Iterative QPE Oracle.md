#Q-Sharp #Algorithm 
[[Iterative QPE]] reuses single control qubit across all $t$ iterations. [[Oracle]] structure is identical; what changes is that measurement results from previous iterations feed forward as classical corrections:
```csharp
import Std.Math.*;
import Std.Canon.*;

@Config(AdaptiveRI)
operation IterativeQPEOracle(U : (Qubit => Unit is Adj + Ctl),
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

Source: [microsoft/qsharp - PhaseEstimation.qs](https://github.com/microsoft/qsharp/blob/main/samples/algorithms/PhaseEstimation.qs) · [Microsoft Learn - Quantum phase estimation concepts](https://learn.microsoft.com/en-us/azure/quantum/concepts-phase-estimation)
#Algorithm #Q-Sharp [[Grover]] Search ($n = 20$, single solution)
~$\sqrt{2^{20}} \approx 1024$ iterations. Each iteration contains multi-controlled $X$ ($T$-ladder cost).
```csharp
import Std.Arrays.*;
import Std.Canon.*;
import Std.Math.*;

operation GroverEstimate() : Unit {
    let n = 20;
    let target = 42;
    let nIter = Round(PI() / 4.0 * Sqrt(IntAsDouble(1 <<< n)));
    use register = Qubit[n];
    ApplyToEach(H, register);
    for _ in 1..nIter {
        use anc = Qubit();
        within { X(anc); H(anc); }
        apply  { ControlledOnInt(target, X)(register, anc); }
        within { ApplyToEach(H, register); ApplyToEach(X, register); }
        apply  { Controlled Z(Most(register), Tail(register)); }
    }
    ResetAll(register);
}
```
**Expected output** *(rough illustrative estimates from Azure [[QRE]] - depend on error model, code distance, and target logical error rate):*

| Field            | Approximate value  |
| ---------------- | ------------------ |
| `physicalQubits` | $~100,000-500,000$ |
| `logicalQubits`  | $~200-400$         |
| `tStates`        | $~50,000-500,000$  |
| `tFactories`     | $10-50$ parallel   |
| `runtime`        | seconds to minutes |
| `codeDistance`   | $13-17$            |
With $1024$ iterations & $~100$ T-gates/controlled $X$, total $T$-count is $~10^5$. This is what 'quadratic speedup' costs in physical resources.


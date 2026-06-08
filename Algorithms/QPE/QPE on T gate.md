#Algorithm #Q-Sharp [[QPE]] on $T$-Gate ($t = 10$ phase bits)
[[QPE]] estimates the phase $\varphi = 1/8$ of T's eigenvalue. Requires $\sum_{k=0}^{9} 2^k = 1023$ apps of $T$.
```csharp
import Std.Canon.*;

operation QPEEstimate() : Unit {
    let t = 10;
    use (phaseReg, eigenstate) = (Qubit[t], Qubit());
    X(eigenstate);
    ApplyToEach(H, phaseReg);
    for k in 0..t-1 {
        let Tk = OperationPow(T, 1 <<< k);
        Controlled Tk([phaseReg[k]], eigenstate);
    }
    Adjoint ApplyQFT(phaseReg);
    ResetAll(phaseReg + [eigenstate]);
}
```

*Rough illustrative estimates from Azure [[QRE]] - exact values depend on error model, code distance, & target logical error rate.*

| Field            | Expected Output Value   |
| ---------------- | ----------------------- |
| `physicalQubits` | $~50,000-200,000$       |
| `tStates`        | $1023 (2^{10}-1)$       |
| `runtime`        | milliseconds to seconds |
| `codeDistance`   | $11-15$                 |
$T$-count scales as $2^t - 1$. For $t=20$ precision bits: ~$10^6$ T-gates, millions of physical [[Qubits]].

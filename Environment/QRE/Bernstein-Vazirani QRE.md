#Algorithm #Q-Sharp 
[[Bernstein-Vazirani]] ($n = 20$).
$BV$ is Clifford-only: $1$ $H$ layer, $1$ [[CNOT]] layer, $1$ $H$ layer. $0$ [[T-gates]].
```csharp
import Std.Arrays.*;
import Std.Convert.*;

operation BVEstimate() : Unit {
    let n = 20;
    let s = 0b10110100110101011010;
    use (input, anc) = (Qubit[n], Qubit());
    X(anc); H(anc);
    ApplyToEach(H, input);
    let bits = IntAsBoolArray(s, n);
    for i in 0..n-1 { if bits[i] { CNOT(input[i], anc); } }
    ApplyToEach(H, input);
    ResetAll(input + [anc]);
}
```

**Expected output (superconducting `qubit_gate_ns_e3`, [[Surface Code]]):**

| Field            | Value             |
| ---------------- | ----------------- |
| `physicalQubits` | $~500-2000$       |
| `logicalQubits`  | $~22$             |
| `tStates`        | 0 (Clifford only) |
| `tFactories`     | $0$               |
| `runtime`        | microseconds      |
| `codeDistance`   | $5-7$             |
$0$ [[T-gates]] means $0$ T-factory overhead. BV is baseline test any near-term device can run.

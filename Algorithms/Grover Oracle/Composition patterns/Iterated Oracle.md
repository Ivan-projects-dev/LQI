#Q-Sharp #Algorithm Iterated [[Oracle]] ($U^{2^k}$ for [[QPE]])
[[QPE]] requires applying aunitary $U$ exactly $2^k$ times, controlled on a single qubit. Use `OperationPow` from `Std.Canon`:
```csharp
import Std.Canon.*;

operation ApplyUPow(U : Qubit => Unit is Adj + Ctl,
    k : Int,
    control : Qubit,
    target : Qubit) : Unit is Adj + Ctl {
    let Uk = OperationPow(U, 1 <<< k); // U^(2^k)
    Controlled Uk([control], target);
}
```
For custom unitary (not single-gate), wrap it in operation & pass it as $1st$-class value:
```csharp
import Std.Canon.*;

operation PhaseKickbackU(eigenstate : Qubit, phase : Double) : Unit is Adj + Ctl {
    Rz(phase, eigenstate); // simplified: single-qubit phase rotation as U
}

operation QPEOracle(k : Int, control : Qubit, eigenstate : Qubit) : Unit is Adj + Ctl {
    let U = OperationPow(PhaseKickbackU(_, 0.5), 1 <<< k);
    Controlled U([control], eigenstate);
}
```
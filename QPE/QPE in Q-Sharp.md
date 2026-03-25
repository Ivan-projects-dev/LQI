#Q-Sharp #Quantum #Algorithm
Complete Q# implementation of [[QPE]]. Estimates the **eigenphase** $\varphi$ of a unitary $U$ given eigenstate $|u\rangle$, returning $t$-bit approximation $\hat{\varphi} \approx \varphi$.

$3$ stages: superposition on control register → controlled-$U^{2^j}$ → inverse QFT.
```csharp
import Std.Diagnostics.*;
import Std.Math.*;
import Std.Measurement.*;
import Std.Arrays.*;

operation EstimatePhase(
    nControlBits : Int, // t: precision bits
    eigenstate : Qubit[], // |u⟩: target register
    controlledU : (Int, Qubit[]) => Unit is Adj + Ctl // applies U^power
) : Double {
    use control = Qubit[nControlBits];
    // uniform superposition on control register
    ApplyToEach(H, control);

    // controlled-U^(2^j) for each control qubit j
    for j in 0..nControlBits - 1 {
        let power = 1 <<< j; // 2^j
        Controlled controlledU([control[j]], (power, eigenstate));
    }

    // inverse QFT on control register
    Adjoint ApplyQFT(control);

    // Measure & decode
    let result = MResetEachZ(control);
    return IntAsDouble(ResultArrayAsIntBE(result)) / IntAsDouble(1 <<< nControlBits);
}
```
`controlledU(power, eigenstate)` applies $U^{\text{power}}$ to `eigenstate`. Caller constructs this from their specific unitary.

Q# STL provides `ApplyQFT` in `Std.Arithmetic`. If implementing manually:
```csharp
operation ApplyQFT(register : Qubit[]) : Unit is Adj + Ctl {
    let n = Length(register);
    for i in 0..n - 1 {
        H(register[i]);
        for j in i + 1..n - 1 {
            let angle = 2.0 * PI() / IntAsDouble(1 <<< (j - i + 1));
            Controlled R1([register[j]], (angle, register[i]));
        }
    }
    // Reverse qubit order (QFT output is bit-reversed)
    for i in 0..n / 2 - 1 {
        SWAP(register[i], register[n - 1 - i]);
    }
}
// Adjoint ApplyQFT is auto-generated - that is QFT⁻¹ used in QPE stage 3
```
`R1(θ, q)` applies phase $e^{i\theta}$ to $|1\rangle$ component. `Controlled R1([ctrl], (θ, q))` is the controlled phase gate $CR_k$ with $\theta = 2\pi/2^k$.

Simplest implementation applies $U$ sequentially $2^j$ times:
```csharp
operation ApplyUPower(
    applyU  : Qubit[] => Unit is Adj + Ctl,
    power   : Int,
    target  : Qubit[]
) : Unit is Adj + Ctl {
    for _ in 1..power {
        applyU(target);
    }
}
```
This costs $O(2^j)$ per stage → $O(2^t)$ total $U$ apps for $t$ control [[Qubits]]. For structured unitaries (e.g. $U = e^{-iHt}$), implement `applyU` as the Trotter approximation & use repeated squaring to reduce cost.

**Full [[QPE]] entry point example**
```csharp
@EntryPoint()
operation Main() : Unit {
    // Example: estimate eigenphase of T gate (T|1⟩ = e^{iπ/4}|1⟩, φ = 1/8)
    let nBits = 4;                // 4-bit precision → φ accurate to 1/16
    use eigenstate = Qubit();
    X(eigenstate);                // prepare |1⟩ (eigenstate of T)

    let phaseInt = IterativePhaseEstimation(nBits, [eigenstate], q => T(q[0]));
    let phaseEst = IntAsDouble(phaseInt) / IntAsDouble(1 <<< nBits);

    Message($"Estimated φ = {phaseEst}");  // expect ≈ 0.125 = 1/8
    Reset(eigenstate);
}
```
`T` gate: eigenphase $\varphi = 1/8$ → binary $0.0010$ → `phaseInt` = $2$ out of $16$.

| Method | Control [[Qubits]] | $U$ apps | Rounds |
|---|---|---|---|
| Standard [[QPE]] | $t$ | $2^t - 1$ | 1 (parallel) |
| Iterative [[QPE]] | $1$ | $2^t - 1$ | $t$ (sequential) |
| [[Quantum counting]] | $t$ | $O(2^t)$ calls to $G$ | 1 |

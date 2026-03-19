#Quantum #Q-Sharp 
**Iterative [[QPE]]** ($1$-qubit version, Kitaev method)

Extracts $\varphi$ one bit/time, using only $1$ control qubit. Lower qubit overhead, but $t$ sequential rounds:
```csharp
operation IterativePhaseEstimation(
    nBits   : Int,
    target  : Qubit[],
    applyU  : Qubit[] => Unit is Adj + Ctl
) : Int {
    mutable phase = 0;
    use control = Qubit();

    for i in nBits - 1..-1..0 {            // from MSB to LSB
        H(control);
        let power = 1 <<< i;
        Controlled applyU([control], target);

        // Correct for previously estimated bits
        let angle = -2.0 * PI() * IntAsDouble(phase) / IntAsDouble(1 <<< (nBits - i));
        R1(angle, control);

        H(control);
        if M(control) == One { set phase += 1 <<< i; }
        Reset(control);
    }
    return phase;
}
```
Each round: $1$ Hadamard, controlled-$U^{2^i}$, phase correction $R_1$, second Hadamard, measure. The phase correction `R1(angle, control)` removes the contribution of already-known bits before the final Hadamard collapses the control.

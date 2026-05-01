#Algorithm #Math **Kitaev's Phase Estimation Approach (Iterative [[QPE]])**

Instead of using $t$ [[Qubits]] in the phase register simultaneously, use **single control qubit** & repeat $t$ times, using classical feedback between measurements.

This reformulation of [[Shor]] uses the **semi-classical QFT**: each bit of the phase is extracted one at time by measuring the control qubit & conditioning the next rotation on the measurement outcome.
```csharp
import Std.Canon.*;

// Iterative QPE: extract one phase bit per call using a single control qubit
// Uses feed-forward (AdaptiveRI profile required)
operation IterativePhaseBit(U : (Qubit => Unit is Adj + Ctl),
    target : Qubit,
    k : Int, // bit position (from LSB = 0)
    prevBits : Result[] // previously measured bits (for feed-forward rotation)
) : Result {
    use control = Qubit();
    H(control);
    
    // Controlled U^(2^k) - applies U exactly 2^k times
    for _ in 1..(1 <<< k) {
        Controlled U([control], target);
    }

    // Semi-classical QFT: undo phase accumulated by already-measured bits
    // Apply R_z(-pi * prevBits[j] / 2^(k-j)) for each previously measured bit j
    mutable angle = 0.0;
    
    for j in 0..k-1 {
        if prevBits[j] == One {
            set angle += -Microsoft.Quantum.Math.PI() / IntAsDouble(1 <<< (k - j));
        }
    }
    Rz(angle, control);
    H(control);
    return MReset(control);
}
```
**Advantage over standard [[Shor]]:** Requires only $O(1)$ [[Qubits]] in the control register ($1$ qubit at time) $+$ target register. Reduces qubit count at the cost of $>$ circuit runs.

**Disadvantage:** Each run extracts only $1$ bit. Requires $O(n)$ runs total, each with $O(2^k)$ controlled-$U$ applications. Total [[Oracle]] calls are the same order as standard [[Shor]].
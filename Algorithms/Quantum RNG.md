#Q-Sharp #Algorithm 
**Quantum random number generation (QRNG)** produces certified random bits by measuring [[Qubits]] in superposition. Randomness is physical - measurement outcomes are fundamentally unpredictable, not computed from a seed.

**Single random bit**:  $H|0\rangle = |+\rangle$ collapses to $|0\rangle$ or $|1\rangle$ with equal probability:
```csharp
operation RandomBit() : Result {
    use q = Qubit();
    H(q);
    return MResetZ(q);
}
```

**Random int in $[0,\, 2^n - 1]$**
```csharp
import Std.Convert.*;

operation RandomInt(nBits : Int) : Int {
    use qs = Qubit[nBits];
    ApplyToEach(H, qs);
    let results = MResetEachZ(qs);
    return ResultArrayAsIntBE(results);
}
```

**Random int in arbitrary range $[\text{min},\, \text{max}]$**

**Rejection sampling**: find min $n$ with $2^n \geq \text{max} - \text{min} + 1$, sample from $[0, 2^n - 1]$, retry if out of range.
```csharp
import Std.Math.*;

operation RandomIntInRange(min : Int, max : Int) : Int {
    Fact(max >= min, "max must be >= min");
    let range = max - min;
    let nBits = Floor(Log(IntAsDouble(range + 1)) / Log(2.0)) + 1;
    mutable sample = range + 1;
    repeat {
        set sample = RandomInt(nBits);
    } until sample <= range;
    return min + sample;
}
```
Expected iterations: $\leq 2$ by construction of `nBits`.

**Random double in $[0, 1)$**: Sample $53$ bits (IEEE $754$ mantissa width) & normalize:
```csharp
operation RandomDouble() : Double {
    return IntAsDouble(RandomInt(53)) / IntAsDouble(1 <<< 53);
}
```

`Std.Random` provides classical PRNG funcs usable inside `operation` or `function`:
```csharp
import Std.Random.*;

let n = DrawRandomInt(0, 100);
let p = DrawRandomDouble(0.0, 1.0);
let b = DrawRandomBool(0.75); // true with prob 0.75
```
Deterministic given seed - not truly random. Use quantum sampling when genuine unpredictability is required ([[QKD]], Monte Carlo, online protocols).

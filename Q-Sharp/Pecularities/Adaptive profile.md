#Q-Sharp #Quantum
**Adaptive profile** (`AdaptiveRI`) enables **classical feed-forward**: using mid-circuit measurement results to conditionally apply subsequent quantum ops. Essential for [[FTQC|fault-tolerant]] protocols, magic state distillation, & [[QPE|iterative QPE]].

Q# defines $3$ exec profiles, selected via `@Config` attribute or target machine setting:

| Profile | Gate set | Mid-circuit measurement | Feed-forward |
|---|---|---|---|
| `Base` | Clifford + T | Allowed | No |
| `AdaptiveRI` | Clifford + T + rotations | Allowed | Yes |
| `Unrestricted` | Any gate | Allowed | Yes |

**`AdaptiveRI` feed-forward pattern**
```csharp
@Config(AdaptiveRI)
operation TeleportQubit(msg : Qubit, here : Qubit) : Unit {
    use there = Qubit();
    H(there);
    CNOT(there, here);
    CNOT(msg, there);
    H(msg);
    let m1 = M(msg);
    let m2 = M(there);
    if m2 == One { X(here); }     // feed-forward: conditioned on m2
    if m1 == One { Z(here); }     // feed-forward: conditioned on m1
    Reset(msg);
    Reset(there);
}
```
Both `if` branches are **real-time conditional gates** - the classical result drives the hardware in the same shot, without restarting. On `Base` profile this code would fail to compile.

**Iterative [[Iterative QPE|QPE]]** is the canonical use case. Estimates phase one bit at a time, each iteration conditioned on all prior measurement results:
```csharp
@Config(AdaptiveRI)
operation IterativePhaseEstimation(oracle : Qubit => Unit is Adj + Ctl, n : Int) : Int {
    mutable phase = 0;
    use q = Qubit();
    for k in n-1..-1..0 {
        H(q);
        // apply oracle 2^k times, controlled on q
        for _ in 1..1 <<< k { Controlled oracle([q], target); }
        // feed-forward: apply phase correction based on prior bits
        Rz(-2.0 * PI() * IntAsDouble(phase) / IntAsDouble(1 <<< (n - k)), q);
        H(q);
        let r = MResetZ(q);
        if r == One { set phase w/= k <- 1; }  // update known bits
    }
    return phase;
}
```

**`ResultAsBool` & classical [[Logic]] on `Result`**
```csharp
let b = ResultAsBool(M(q)); // Result → Bool for arithmetic
let combined = m1 == One and m2 == Zero;
if combined { Z(target); }
```

**Hardware constraints on `AdaptiveRI`**
- Feed-forward latency matters on real hardware. Classical processing between measurement & next gate adds delay - some QPUs impose deadtime limits.
- Only `if`/`elif`/`else` on `Result` values are feed-forward; classical `Bool` conditions are always allowed on all profiles.
- `while` & `repeat-until` with `Result` conditions require `AdaptiveRI` on hardware.
- Operations inside conditional branches must be **`Ctl`-enabled** if they rely on the result.

**`@Config` stacking for simulation compatibility**
Define $2$ versions of the same operation - one for restricted hardware, one for simulation:
```csharp
@Config(Base)
operation MeasureAndCorrect(q : Qubit) : Unit {
    // Base: no feed-forward, use non-adaptive approach
    Reset(q);
}

@Config(AdaptiveRI)
operation MeasureAndCorrect(q : Qubit) : Unit {
    let r = M(q);
    if r == One { X(q); }   // adaptive correction
}
```
Compiler selects the matching variant based on the active profile. See [[Q-Sharp attributes]] for `@Config` syntax.

Classical `Bool` conditions → resolved at compile time or pre-circuit classical compute.
`Result` conditions on `AdaptiveRI` → resolved mid-circuit in real time on hardware.
`Result` conditions on `Base` → compile error if feed-forward required.

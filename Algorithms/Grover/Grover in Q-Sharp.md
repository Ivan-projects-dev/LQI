#Q-Sharp #Quantum #Algorithm
Complete Q# implementation of [[Grover]]'s search algorithm. Combines [[Grover]] [[Oracle]] in Q-Sharp, [[Diffusion operator]] in Q-Sharp, & the outer loop with iteration count [[Logic]].

**Single [[Grover]] iteration**
One app of $G = D \cdot U_f$:
```csharp
operation GroverIteration(
    register      : Qubit[],
    markingOracle : (Qubit[], Qubit) => Unit is Adj
) : Unit is Adj {
    // Phase oracle: apply marking oracle with ancilla in |−⟩
    use target = Qubit();
    within {
        X(target);
        H(target);
    } apply {
        markingOracle(register, target);
    }
    // Diffusion operator
    ApplyGroverDiffusion(register);
}
```

**Fixed-iteration [[Grover search]]** (known $M$)
```csharp
operation RunGroverSearch(
    nQubits       : Int,
    nSolutions    : Int,                              // M
    markingOracle : (Qubit[], Qubit) => Unit is Adj
) : Result[] {
    let nIterations = GroverIterationCount(nQubits, nSolutions);
    use register = Qubit[nQubits];
    ApplyToEach(H, register);                         // uniform superposition
    for _ in 1..nIterations {
        GroverIteration(register, markingOracle);
    }
    return MResetEachZ(register);                     // measure & reset
}

function GroverIterationCount(n : Int, m : Int) : Int {
    // k* = floor(π/4 · √(N/M))
    let n_f = IntAsDouble(1 <<< n);   // N = 2^n
    let m_f = IntAsDouble(m);
    return Floor(PI() / 4.0 * Sqrt(n_f / m_f));
}
```
`MResetEachZ` from `Std.Measurement` measures each qubit in Z-basis & resets it.

**[[Grover search]] with unknown $M$ - exponential search**

When $M$ is unknown, try geometrically increasing iteration counts until a solution is found:
```csharp
operation RunGroverUnknownM(nQubits : Int, markingOracle : (Qubit[], Qubit) => Unit is Adj, 
	classicalCheck: Int[] -> Bool // verify solution classically
) : Int[] {
    mutable found = false;
    mutable solution = [];
    mutable maxIter = 1;

    repeat {
        // Pick random k in [1, maxIter]
        let k = DrawRandomInt(1, maxIter);
        use register = Qubit[nQubits];
        ApplyToEach(H, register);
        for _ in 1..k {
            GroverIteration(register, markingOracle);
        }
        let result = MResetEachZ(register);
        let bits = ResultArrayAsIntBE(result);        // convert to int

        if classicalCheck([bits]) {
            set found = true;
            set solution = [bits];
        }
        set maxIter = MinI(2 * maxIter, 1 <<< nQubits);
    } until found;

    return solution;
}
```
Expected [[Oracle]] calls: $O(\sqrt{2^n / M})$ - matches the fixed-$M$ complexity without knowing $M$.

**Classical result verification**

Always verify the measurement result classically (evaluating $f$ once) before returning. [[Grover]] succeeds with high probability, not certainty:
```csharp
function IsValidSolution(x : Int, oracle : Int -> Bool) : Bool {
    return oracle(x);
}
```
If the result fails verification (probability $\leq M/N$), restart the quantum search.

**Full runnable entry point**
```csharp
import Std.Measurement.*;
import Std.Math.*;
import Std.Arrays.*;
import Std.Convert.*;

@EntryPoint()
operation Main() : Unit {
    let nQubits = 4; // search space: 2^4 = 16 elements
    let target = 11; // the solution we want to find

    let markingOracle = (register : Qubit[], ancilla : Qubit) => {
        ControlledOnInt(target, X)(register, ancilla);
    };

    let result = RunGroverSearch(nQubits, 1, markingOracle);
    let found = ResultArrayAsIntBE(result);
    Message($"Found: {found}"); // should print 11
}
```
**Resource summary for $N = 2^n$, $M$ solutions**

| Resource            | Value                                   |
| ------------------- | --------------------------------------- |
| [[Qubits]]              | $n + 1$ (register + [[Ancilla]])            |
| Iterations          | $\lfloor\frac{\pi}{4}\sqrt{N/M}\rfloor$ |
| Gates / iteration   | $O(n) + $ [[Oracle]] cost                   |
| T-gates / diffusion | $O(n)$ Toffoli decomposition            |
| Measurements        | $n$ (at the end)                        |

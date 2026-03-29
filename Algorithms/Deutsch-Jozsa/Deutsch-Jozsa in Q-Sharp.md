#Algorithm #Q-Sharp 
**[[Deutsch-Jozsa]]** determines if bool func $f:\{0,1\}^n \to \{0,1\}$ is **constant** (same output for all inputs) or **balanced** (exactly $1/2$ inputs map to $0$, $1/2$ to $1$). Classical deterministic: $2^{n-1}+1$ queries worst case. Quantum: **$1$ query**.

$n$ input [[Qubits]] start in $|0\rangle^{\otimes n}$, $1$ [[Ancilla]] in $|1\rangle$:
$$|0\rangle^n|1\rangle \xrightarrow{H^{\otimes n+1}} |+\rangle^n|-\rangle \xrightarrow{U_f} (\text{phase-kicked}) \xrightarrow{H^{\otimes n}} \text{measure inputs}$$
Phase kickback from $|-\rangle$ encodes $(-1)^{f(x)}$ into amplitudes. After the second $H$ layer, all amplitude is at $|0\rangle^n$ iff $f$ is constant; $0$ amplitude at $|0\rangle^n$ iff balanced.

**Measurement rule**: all-$0s$ → constant; any non-$0$ bit → balanced.
```csharp
import Std.Arrays.*;

operation DeutschJozsa(n : Int, oracle : (Qubit[], Qubit) => Unit is Adj) : Bool { // true = constant, false = balanced
    use (input, ancilla) = (Qubit[n], Qubit());
    X(ancilla); // |−⟩ ancilla for phase kickback
    H(ancilla);
    ApplyToEach(H, input); // uniform superposition
    oracle(input, ancilla); // single query
    ApplyToEach(H, input); // interfere
    let results = MResetEachZ(input);
    Reset(ancilla);
    return All(r -> r == Zero, results);
}
```

Constant-$0$ [[Oracle]] (do nothing), constant-$1$ [[Oracle]] (flip [[Ancilla]] unconditionally):
```csharp
operation ConstantZeroOracle(input : Qubit[], ancilla : Qubit) : Unit is Adj {
}

operation ConstantOneOracle(input : Qubit[], ancilla : Qubit) : Unit is Adj {
    X(ancilla);
}
```

Balanced parity [[Oracle]] $f(x) = x_0 \oplus \ldots \oplus x_{n-1}$:
```csharp
operation BalancedParityOracle(input : Qubit[], ancilla : Qubit) : Unit is Adj {
    for q in input {
        CNOT(q, ancilla);
    }
}
```

Balanced [[Oracle]] for fixed target bitstring $s$:
```csharp
operation BalancedBitstringOracle(input : Qubit[], ancilla : Qubit, s : Int) : Unit is Adj {
    import Std.Canon.*; // possible
    ApplyControlledOnInt(s, X, input, ancilla);
}
```

```csharp
@EntryPoint()
operation RunDJ() : Unit {
    let n = 4;
    Message(DeutschJozsa(n, BalancedParityOracle) ? "Constant" | "Balanced");
    Message(DeutschJozsa(n, ConstantOneOracle) ? "Constant" | "Balanced");
}
```
$O(1)$ for [[Oracle]] queries. This algorithm is $1st$ proof that quantum computers can solve problem faster than any classical algorithm.

## Sources
- [Deutsch-Jozsa kata (Quantum Katas)](https://quantum.microsoft.com/en-us/tools/quantum-katas)
- [GitHub: Deutsch-Jozsa sample in Q#](https://github.com/microsoft/qsharp/tree/main/samples/algorithms)
- [Q# single-qubit programs tutorial](https://learn.microsoft.com/en-us/azure/quantum/tutorial-qdk-qubit-level-program)

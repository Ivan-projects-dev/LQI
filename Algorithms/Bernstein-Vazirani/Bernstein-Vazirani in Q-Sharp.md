#Q-Sharp #Algorithm 
**[[Bernstein-Vazirani]]** recovers hidden bitstring $s \in \{0,1\}^n$ from black-box [[Oracle]] computing $f(x) = s \cdot x \pmod{2}$ (bitwise inner product). Classical: $n$ queries. Quantum: **$1$ query**.

**[[Oracle]]**: $f(x) = s \cdot x = s_0 x_0 \oplus s_1 x_1 \oplus \ldots \oplus s_{n-1} x_{n-1}$

Implemented as CNOTs from each input qubit $i$ to the [[Ancilla]], for every bit $s_i = 1$.

Phase kickback from $|-\rangle$ imprints $(-1)^{s \cdot x}$ on $|x\rangle$. State after the [[Oracle]] is $H^{\otimes n}|s\rangle$. $2nd$ Hadamard layer yields $|s\rangle$ deterministically - $1$ shot recovers all $n$ bits of $s$.
```csharp
import Std.Arrays.*;
import Std.Convert.*;

operation BVOracle(s : Int, n : Int, input : Qubit[], ancilla : Qubit) : Unit is Adj {
    let bits = IntAsBoolArray(s, n);
    for i in 0..n-1 {
        if bits[i] {
            CNOT(input[i], ancilla);
        }
    }
}

operation BernsteinVazirani(n : Int, s : Int) : Int {
    use (input, ancilla) = (Qubit[n], Qubit());
    X(ancilla);
    H(ancilla);
    ApplyToEach(H, input);
    BVOracle(s, n, input, ancilla);
    ApplyToEach(H, input);
    let results = MResetEachZ(input);
    Reset(ancilla);
    return ResultArrayAsIntBE(results);
}
```

```csharp
@EntryPoint()
operation RunBV() : Unit {
    let n = 6;
    let secret = 0b101011;
    let found  = BernsteinVazirani(n, secret);
    Message($"Secret: {secret}, Recovered: {found}, OK: {found == secret}");
}
```
Same circuit structure ([[Deutsch-Jozsa in Q-Sharp]]). [[Deutsch-Jozsa]] classifies func (constant vs. balanced); [[Bernstein-Vazirani]] extracts hidden parameter. [[Bernstein-Vazirani]] is "learning" problem - $n$ bits of info recovered in $1$ [[Oracle]] call.

## Sources
- [Bernstein-Vazirani kata (Quantum Katas)](https://quantum.microsoft.com/en-us/tools/quantum-katas)
- [GitHub: BV sample in Q#](https://github.com/microsoft/qsharp/tree/main/samples/algorithms)
- [Q# single-qubit programs tutorial](https://learn.microsoft.com/en-us/azure/quantum/tutorial-qdk-qubit-level-program)

#Algorithm  #Q-Sharp
[[Grover]] uses $2$ kinds of oracles. **Marking [[Oracle]]**: flips target [[Ancilla]] qubit if query register satisfies condition. **Phase [[Oracle]]**: flips phase of query register if condition holds. The $2$ are interconvertible.
### **[[Oracle]] types**:
**AllOnes** - flip target iff all [[Qubits]] are $|1\rangle$. Single multi-controlled $X$ with all data [[Qubits]] as controls:
```csharp
// Task 1.1 - f(x) = 1 iff x = |11…1⟩
operation Oracle_AllOnes(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    Controlled X(queryRegister, target);
}
```

**AlternatingBits** - flip target iff register is $|1010\ldots\rangle$. Flip odd-indexed [[Qubits]] so the target pattern becomes all-ones, apply AllOnes, uncompute:
```csharp
// Task 1.2 - f(x) = 1 iff x = |1010…⟩
operation Oracle_AlternatingBits(queryRegister : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    within {
        for i in 1..2..Length(queryRegister) - 1 {
            X(queryRegister[i]);  // flip odd positions
        }
    } apply {
        Controlled X(queryRegister, target);
    }
}
```

**ArbitraryPattern** - flip target iff register matches a given `Bool[]` bit pattern. `ControlledOnBitString` handles the $|0\rangle$-qubit pre/post-flipping internally:
```csharp
import Std.Canon.*;

// Task 1.3 - f(x) = 1 iff x matches pattern
operation Oracle_ArbitraryPattern(queryRegister : Qubit[], target : Qubit, pattern : Bool[]) : Unit is Adj + Ctl {
    ControlledOnBitString(pattern, X)(queryRegister, target);
}
```

**[[Oracle]] converter** (marking → phase) via [[Phase kickback]]:
```csharp
// Task 1.4* - wrap marking oracle as phase oracle
operation OracleConverterImpl(
    markingOracle : (Qubit[], Qubit) => Unit is Adj,
    register : Qubit[]
) : Unit is Adj {
    use target = Qubit();
    within {
        X(target);
        H(target);  // |0⟩ → |−⟩
    } apply {
        markingOracle(register, target);  // phase kickback: (−1)^f(x) on register
    }
}

function OracleConverter(
    markingOracle : (Qubit[], Qubit) => Unit is Adj
) : (Qubit[] => Unit is Adj) {
    return OracleConverterImpl(markingOracle, _);
}
```
[[Ancilla]] returns to $|{-}\rangle$ unchanged; the phase $(-1)^{f(x)}$ is kicked back to $|x\rangle$. [[Ancilla]] is never measured and can be reused across iterations.

**[[Grover]] iteration - 4 steps** (from [[Oracle]] to diffusion):
1. Apply phase [[Oracle]] $U_f$ → marks solution(s) with $-1$.
2. Apply $H^{\otimes n}$ to query register.
3. Apply **conditional phase flip** $2|0\rangle\langle 0| - I$: flip sign of every state except $|0\ldots0\rangle$.
4. Apply $H^{\otimes n}$ again.
Steps $2$–$4$ together form the [[Diffusion operator]] $D = H^{\otimes n}(2|0\rangle\langle 0|-I)H^{\otimes n}$

Source: [microsoft/QuantumKatas - GroversAlgorithm/ReferenceImplementation.qs (Tasks 1.1–1.4)](https://github.com/microsoft/QuantumKatas/blob/main/GroversAlgorithm/ReferenceImplementation.qs)

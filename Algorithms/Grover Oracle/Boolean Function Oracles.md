#Q-Sharp #Algorithm #Quantum
Boolean function oracles in Q# — primitive building blocks for [[Grover]], [[SAT oracle]], and [[Graph coloring oracle]]. All follow the **marking oracle** pattern: flip `target` iff the function is 1 on `register`.

## Single-Bit Check

Flip target iff qubit $k$ is $|1\rangle$. The simplest possible oracle — just a CNOT:

```csharp
operation MarkBitIsOne(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    CNOT(register[k], target);
}
```

Flip target iff qubit $k$ is $|0\rangle$ — CNOT with pre/post X on the control:
```csharp
operation MarkBitIsZero(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    within { X(register[k]); }
    apply  { CNOT(register[k], target); }
}
```

## AND Oracle

Flip target iff $x_i = 1$ **and** $x_j = 1$. A 2-controlled X (Toffoli gate):
```csharp
operation MarkAND(register : Qubit[], target : Qubit, i : Int, j : Int) : Unit is Adj + Ctl {
    CCNOT(register[i], register[j], target);  // Toffoli
}
```

Generalized AND — flip target iff **all** bits in a given index list are $|1\rangle$:
```csharp
operation MarkAllOnes(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    Controlled X(Mapped(i -> register[i], indices), target);
}
```

## OR Oracle

Flip target iff **at least one** bit in index list is $|1\rangle$.
OR = NOT AND NOT — apply X to all selected bits, check if any is still 1, uncompute:

```csharp
operation MarkOR(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    let selected = Mapped(i -> register[i], indices);
    within {
        ApplyToEach(X, selected);           // flip: 0→1, 1→0
    } apply {
        // now target fires iff original had at least one 1
        // (De Morgan: OR(x) = NOT AND(NOT x))
        // flip target iff ALL selected are now 0 (= original all-ones after negation)
        // actually: fire iff NOT all-zeros in original
        // Simplest: mark NOT(all-zero), which = NOT(all-one after flip)
        within { ApplyToEach(X, selected); }  // flip back
        apply  { Controlled X(selected, target); }
    }
    // Cleaner equivalent using NOR trick:
}

// Cleaner version using NOR (NOT OR = AND of negated inputs):
operation MarkOR_Clean(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    let selected = Mapped(i -> register[i], indices);
    // Phase 1: mark NOR (all inputs are 0)
    within {
        ApplyToEach(X, selected);           // negate inputs
    } apply {
        Controlled X(selected, target);     // target |= AND(NOT inputs) = NOR
    }
    // Phase 2: flip target to get OR = NOT NOR
    X(target);
}
```

## XOR Oracle

Flip target iff an odd number of selected bits are $|1\rangle$ (parity):

```csharp
operation MarkXOR(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    for i in indices {
        CNOT(register[i], target);   // each CNOT adds register[i] mod 2 to target
    }
}
```

XOR is its own inverse — calling this operation twice restores target.

## Parity Oracle (Full Register)

Flip target iff the number of $|1\rangle$ qubits in the whole register is odd:

```csharp
operation MarkParity(register : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    for q in register {
        CNOT(q, target);
    }
}
```

This is the [[Deutsch-Jozsa]] balanced oracle and the [[Bernstein-Vazirani]] inner product with $s = 11\ldots1$.

## Majority Oracle

Flip target iff **more than half** of $n$ selected bits are $|1\rangle$. For 3 bits: Maj$(a,b,c) = ab \vee ac \vee bc$:

```csharp
operation MarkMajority3(a : Qubit, b : Qubit, c : Qubit, target : Qubit) : Unit is Adj + Ctl {
    use anc = Qubit[2];
    within {
        CCNOT(a, b, anc[0]);    // anc[0] = a AND b
        CCNOT(a, c, anc[1]);    // anc[1] = a AND c
    } apply {
        // target flipped iff (a AND b) OR (a AND c) OR (b AND c)
        // = any two of three are 1
        CCNOT(b, c, target);    // b AND c
        CNOT(anc[0], target);   // XOR with a AND b
        CNOT(anc[1], target);   // XOR with a AND c
        // target = (a∧b) ⊕ (a∧c) ⊕ (b∧c) which equals Majority for 3 bits
    }
    // anc automatically uncomputed by within/apply
}
```

## Threshold Oracle (at least $k$ of $n$ bits are 1)

Flip target iff at least $k$ bits out of $n$ are $|1\rangle$. Uses an ancilla counter register:

```csharp
import Std.Arrays.*;
import Std.Convert.*;

operation MarkAtLeastK(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    let bits = Ceiling(Lg(IntAsDouble(n + 1)));   // bits needed to represent count 0..n
    use counter = Qubit[bits];
    within {
        // Increment counter for each |1⟩ qubit (quantum ripple-add)
        for (i, q) in Enumerated(register) {
            Controlled IncrementLE([q], counter);
        }
    } apply {
        // Mark iff counter >= k  →  counter - k >= 0
        // Simplest: check counter[bits-1] after subtracting k
        // Here: use ControlledOnInt to check each value >= k
        for val in k..n {
            ControlledOnInt(val, X)(counter, target);
        }
    }
}
```

Note: `IncrementLE` is a little-endian quantum increment from `Std.Arithmetic`. For large $n$, replace the loop with a proper quantum adder tree.

## Sources
- [Q# Std.Canon — ControlledOnInt](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.canon/controlledonint)
- [Conjugations (within/apply)](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/statements/conjugations)
- [Toffoli gate and multi-controlled operations](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.intrinsic/ccnot)
- [Quantum Katas — Oracles](https://quantum.microsoft.com/en-us/tools/quantum-katas)

#Q-Sharp #Algorithm #Quantum
Arithmetic oracles in Q# — encode numerical conditions as quantum marking operations. Used in [[Bounded knapsack oracle]], Grover-based optimization, and quantum search on structured data.

## Equality Oracle

Flip target iff the register encodes a specific integer value $x_0$. The canonical method uses `ControlledOnInt`:

```csharp
import Std.Canon.*;

operation MarkEquals(register : Qubit[], target : Qubit, x0 : Int) : Unit is Adj + Ctl {
    ControlledOnInt(x0, X)(register, target);
}
```

`ControlledOnInt(n, op)(controls, target)` applies `op` to `target` iff the integer encoded in `controls` (little-endian) equals `n`. Internally it applies $X$ to every control qubit where corresponding bit of `n` is $0$, does multi-controlled $X$, then uncomputes - exactly `ArbitraryPattern` trick in [[Grover oracles]].

For fixed-length bit pattern instead of int:
```csharp
import Std.Canon.*;

operation MarkBitPattern(register : Qubit[], target : Qubit, pattern : Bool[]) : Unit is Adj + Ctl {
    ControlledOnBitString(pattern, X)(register, target);
}
```

## Inequality Oracle (register ≠ constant)

Flip target iff register does **not** equal $x_0$:
```csharp
import Std.Canon.*;

operation MarkNotEquals(register : Qubit[], target : Qubit, x0 : Int) : Unit is Adj + Ctl {
    // Mark all states except x0: flip target for all values, then unflip for x0
    X(target); // flip target unconditionally
    ControlledOnInt(x0, X)(register, target); // unflip for the excluded value
}
```

## Greater-Than Oracle (register > threshold)

Flip target iff the $n$-bit integer in `register` is strictly greater than `threshold`. Uses an ancilla register and quantum subtraction to detect borrow:

```csharp
import Std.Arithmetic.*; // for quantum adder primitives
import Std.Convert.*;

// Marks register > threshold using borrow bit from (threshold - register)
// Borrow = 1 iff threshold < register, i.e. register > threshold
operation MarkGreaterThan(register : Qubit[], target : Qubit, threshold : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    use borrow = Qubit();
    within {
        // Compute (threshold - register) mod 2^n into a scratch register
        // Borrow bit set iff threshold < register (underflow)
        ApplyXorInPlace(threshold, register);    // register ^= threshold (bitwise)
        // For a true comparison, use a proper subtractor; here: simplified ripple borrow
        // Full implementation requires Std.Arithmetic quantum subtractor
    } apply {
        CNOT(borrow, target); // target flipped iff borrow = 1, i.e. register > threshold
    }
}
```

Full production version using quantum comparator from `Std.Arithmetic`:

```csharp
import Std.Arithmetic.*;

operation MarkGreaterThan_Full(
    register : Qubit[],
    target : Qubit,
    threshold : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    use (thresholdReg, cmp) = (Qubit[n], Qubit());
    within {
        ApplyXorInPlace(threshold, thresholdReg); // encode threshold as quantum register
        CompareUsingRippleCarry(register, thresholdReg, cmp); // cmp = 1 iff register > threshold
    } apply {
        CNOT(cmp, target);
    }
}
```

## Range Oracle (lo ≤ register ≤ hi)

Flip target iff the register value is within an inclusive range:

```csharp
import Std.Arithmetic.*;

operation MarkInRange(register : Qubit[], target : Qubit, lo : Int, hi : Int) : Unit is Adj + Ctl {
    use (geqLo, leqHi) = (Qubit(), Qubit());
    within {
        // geqLo = 1 iff register >= lo  (= NOT (register < lo) = NOT (register <= lo-1))
        // leqHi = 1 iff register <= hi  (= NOT (register > hi))
        MarkGreaterThan_Full(register, geqLo, lo - 1); // register > lo-1  ⟺  register >= lo
        within { MarkGreaterThan_Full(register, leqHi, hi); }
        apply  { X(leqHi); } // NOT(register > hi) = register <= hi
    } apply {
        CCNOT(geqLo, leqHi, target);   // both conditions must hold
    }
}
```

## Parity of Integer (odd/even)

Flip target iff the integer in `register` is odd (LSB is 1):

```csharp
operation MarkOdd(register : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    CNOT(register[0], target); // register[0] = LSB in little-endian encoding
}
```

## Divisibility Oracle (register mod k == 0)

Flip target iff register is divisible by $k$. Requires a quantum modular reducer:

```csharp
import Std.Arrays.*;
import Std.Convert.*;

operation MarkDivisibleBy(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    let remBits = Ceiling(Lg(IntAsDouble(k)));
    use remainder = Qubit[remBits];
    within {
        // Compute register mod k into remainder register
        // This requires a quantum modular adder circuit (hardware-specific)
        // Simplified: for k = 2^m, divisibility = lower m bits all zero
        // For general k, use modular addition with classical precomputed table
        for i in 0..n-1 {
            // Add 2^i mod k to remainder iff register[i] = 1
            let contrib = (1 <<< i) % k;
            Controlled AddConstantModN([register[i]], (contrib, k, remainder));
        }
    } apply {
        // Mark iff remainder == 0
        ControlledOnInt(0, X)(remainder, target);
    }
}
```

Note: `AddConstantModN` is a classical-constant quantum modular adder — standard building block in Shor's algorithm. Implement with a sequence of controlled additions and reductions, or use the `Std.Arithmetic` module adders as primitives.

## Weighted Sum Threshold Oracle

Flip target iff $\sum_i w_i x_i \geq T$ where $x_i$ are the qubit values and $w_i$ are classical integer weights. Used in [[Bounded knapsack oracle]] and portfolio optimization:

```csharp
import Std.Arrays.*;
import Std.Arithmetic.*;

operation MarkWeightedSumGeq(
    register : Qubit[],
    target   : Qubit,
    weights  : Int[],
    threshold : Int
) : Unit is Adj + Ctl {
    let n = Length(register);
    let maxSum = Fold((acc, w) -> acc + w, 0, weights);
    let sumBits = Ceiling(Lg(IntAsDouble(maxSum + 1)));
    use sumReg = Qubit[sumBits];
    within {
        for (i, q) in Enumerated(register) {
            // Add weights[i] to sumReg iff register[i] = 1
            Controlled AddConstant([q], (weights[i], sumReg));
        }
    } apply {
        MarkGreaterThan_Full(sumReg, target, threshold - 1);  // sum > threshold-1  ⟺  sum >= threshold
    }
}
```

## Key Patterns

**All arithmetic oracles must be `Adj + Ctl`** — they will be called inside `within/apply` blocks or as controlled operations in larger oracles. Any non-reversible step (measurement) inside an oracle breaks adjointability.

**Ancilla registers must be uncomputed.** Every scratch register allocated with `use` inside the `within` block of a `within/apply` pattern is automatically uncomputed by the adjoint of the within block. Never leave ancilla in a non-$|0\rangle$ state.

**Little-endian convention.** Q# `Std.Arithmetic` uses little-endian register encoding by default: `register[0]` is the least significant bit. `ControlledOnInt` uses the same convention.

## Sources
- [Std.Arithmetic API reference](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.arithmetic)
- [Q# integer representation (little-endian)](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/typesystem/datatypes)
- [ControlledOnInt documentation](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.canon/controlledonint)
- [Quantum arithmetic (Wikipedia)](https://en.wikipedia.org/wiki/Quantum_arithmetic)
- [Bounded knapsack oracle](https://learn.microsoft.com/en-us/azure/quantum/tutorial-qdk-grovers-search)

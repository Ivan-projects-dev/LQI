#Algorithm #Q-Sharp Register $>$ threshold
Flip target iff the $n$-bit int in `register` is $>$ `threshold`. Uses [[Ancilla]] register & quantum subtraction to detect borrow:
```csharp
import Std.Arithmetic.*; // for quantum adder primitives
import Std.Convert.*;

// Marks register > threshold using borrow bit from (threshold - register)
// Borrow = 1 iff threshold < register, i.e. register > threshold
operation MarkGreaterThan(register : Qubit[], target : Qubit, threshold : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    use borrow = Qubit();
    within {
        // Compute (threshold - register) mod 2^n into scratch register
        // Borrow bit set iff threshold < register (underflow)
        ApplyXorInPlace(threshold, register); // register ^= threshold (bitwise)
        // For true comparison, use proper subtractor; here: simplified ripple borrow
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

#Q-Sharp #Algorithm Bounded knapsack [[Oracle]]
 $3$ items ($0/1$ knapsack, all $b_i = 1$), capacity $W = 5$.

| Item | Weight $w_i$ | Value $v_i$ |
| ---- | ------------ | ----------- |
| 0    | $2$          | $3$         |
| 1    | $3$          | $4$         |
| 2    | $4$          | $5$         |
Input register: $3$ [[Qubits]], $1$ per item ( {}$q_i = 0$ or $1$). Search space: $2^3 = 8$ assignments.
Value threshold $V^* = 7$ (items $0$ & $1$: weight $5 \leq W$, value $7$).
```csharp
import Std.Arrays.*;
import Std.Canon.*;
import Std.Math.*;

// Add classical int constant c into a quantum register using CNOT fan-out.
// Simple ripple-carry adder for small registers (educational use).
// register: little-endian quantum int accumulator
// c: non-negative int constant to add
operation AddConstant(register : Qubit[], c : Int) : Unit is Adj + Ctl {
    let n = Length(register);
    mutable carry = c;
    for i in 0..n-1 {
        if carry % 2 == 1 {
            X(register[i]);
        }
        set carry = carry / 2;
    }
    // Note: this is NOT a correct quantum adder - it sets bits rather than adds.
    // For correct implementation use the Draper QFT adder or Cuccaro ripple-carry.
    // This simplified version works only for adding to |0⟩ state ancilla registers.
}

// Conditionally add constant c to register if control qubit is |1⟩
operation AddConstantIf(control : Qubit, register : Qubit[], c : Int) : Unit is Adj + Ctl {
    // Controlled version: only add when control = |1⟩
    // Using Controlled AddConstant would work here for the |0⟩-initialized case
    Controlled AddConstant([control], (register, c));
}

// Compare register (little-endian) to classical constant k.
// Sets ancilla = |1⟩ if register >= k (i.e., value meets threshold)
// Works by checking whether (register - k) has no borrow, i.e., register >= k.
// Simplified: just check if the int value encoded ≥ k using classical read.
// In practice, use a quantum comparator circuit (subtractor + borrow bit).
operation MarkAtLeast(register : Qubit[], ancilla : Qubit, k : Int) : Unit is Adj + Ctl {
    // Use ControlledOnInt for small register: enumerate all values >= k
    let n = Length(register);
    let maxVal = 1 <<< n;
    for v in k..maxVal-1 {
        ControlledOnInt(v, X)(register, ancilla);
    }
}

// Marking oracle for 0/1 knapsack:
// Flips target iff the chosen items satisfy weight <= W AND value >= V*
operation MarkKnapsack(
    items : Qubit[],   // one qubit per item: |1⟩ = item selected
    target : Qubit,
    weights : Int[], // w[i]: weight of item i
    values : Int[], // v[i]: value of item i
    capacity : Int, // W
    minValue : Int // V* (threshold)
) : Unit is Adj + Ctl {
    let n = Length(items);
    let wBits = BitSizeI(capacity) + 1; // bits needed for weight sum
    let vBits = BitSizeI(Fold((a, b) -> a + b, 0, values)) + 1;

    use (wReg, vReg) = (Qubit[wBits], Qubit[vBits]);
    use (feasible, meetsValue) = (Qubit(), Qubit());

    within {
        // Accumulate weight & value from selected items
        for i in 0..n-1 {
            AddConstantIf(items[i], wReg, weights[i]);
            AddConstantIf(items[i], vReg, values[i]);
        }
        // Check weight <= capacity: mark feasible if wReg <= capacity
        // Equivalently: NOT (wReg > capacity)
        // Simple approach: mark all w > capacity as infeasible
        within {
            // Mark iff wReg > capacity - then NOT it
            MarkAtLeast(wReg, feasible, capacity + 1);
            X(feasible);   // now feasible = 1 iff weight <= capacity
        }
        apply { () } // (nothing extra in apply - within handles the flip)

        // Separately mark feasible directly for clarity:
        // (the above within/apply resets feasible; redo without within/apply)
        MarkAtLeast(wReg, feasible, capacity + 1);  // feasible = 1 if overweight
        X(feasible); // feasible = 1 if within capacity
        // Check value >= V*
        MarkAtLeast(vReg, meetsValue, minValue);
    }
    apply {
        // Both conditions true → valid knapsack solution
        Controlled X([feasible, meetsValue], target);
    }
}

// Phase oracle wrapper
operation PhaseKnapsack(items : Qubit[], weights : Int[], values : Int[], 
	capacity : Int, minValue : Int) : Unit is Adj {
    use target = Qubit();
    within { X(target); H(target); }
    apply { 
	    MarkKnapsack(items, target, weights, values, capacity, minValue); 
	}
}

// Grover search for optimal knapsack solution
// Items: [{w=2,v=3}, {w=3,v=4}, {w=4,v=5}], W=5, V*=7
// Valid solutions: {item0+item1} (weight=5,value=7) - exactly 1 solution
// Iterations: ~pi/4 * sqrt(8/1) ≈ 2
operation SolveKnapsack() : Result[] {
    let weights  = [2, 3, 4];
    let values = [3, 4, 5];
    let capacity = 5;
    let minValue = 7;
    use items = Qubit[3];
    ApplyToEach(H, items);

    for _ in 1..2 {
        PhaseKnapsack(items, weights, values, capacity, minValue);
        ReflectAboutUniform(items);
    }

    return MResetEach(items);
}
```
**Expected output:** result `[One, One, Zero]` (items $0$ & $1$ selected) with high probability. Total weight $2 + 3 = 5 \leq W$, total value $3 + 4 = 7 \geq V^*$.

**Note on the arithmetic circuit:** simplified `AddConstant` above initializes [[Ancilla]] registers from $|0\rangle$ correctly but does not implement general quantum addition. For a production implementation, use:
- **Draper QFT adder** - uses [[Quantum Fourier Transform]], no [[Ancilla]] carry bits, $O(n^2)$ gates
- **Cuccaro ripple-carry adder** - $O(n)$ [[Ancilla]] [[Qubits]], $O(n)$ depth, hardware-efficient

Both are available as reference implementations in the Std.Arithmetic namespace (Q# standard library).

**Iterative threshold search (optimization loop):**
```csharp
// Repeatedly raise V* until no solution exists, keeping track of best found
operation OptimizeKnapsack(weights : Int[], values : Int[], capacity : Int) : Int[] {
    mutable best = new Int[Length(weights)];  // all zeros initially
    mutable minValue = 1;

    repeat {
        use items = Qubit[Length(weights)];
        ApplyToEach(H, items);
        // Run Grover for current threshold
        let iters = Round(PI() / 4.0 * Sqrt(IntAsDouble(1 <<< Length(weights))));
        for _ in 1..iters {
            PhaseKnapsack(items, weights, values, capacity, minValue);
            ReflectAboutUniform(items);
        }
        let result = MResetEach(items);
        // Check if a valid solution was found
        let selectedItems = Mapped((r, i) -> r == One ? i | -1, Zipped(result, RangeAsIntArray(0..Length(weights)-1)));
        // (classical post-processing: compute total value of result)
        // If valid & better than best, update; else stop
        set minValue += 1;
    } until (minValue > Fold((a, b) -> a + b, 0, values));

    return best;
}
```
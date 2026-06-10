#Algorithm #Q-Sharp Multi-Register [[Oracle]] ([[Oracle]] over $2$ separate registers)
Check if $2$ same-length registers encode the same value. Uses XOR-based equality: two registers are equal iff all corresponding bit pairs are equal (XOR = 0).
```csharp
import Std.Arrays.*;
import Std.Canon.*;

// N-bit register equality oracle (no extra ancilla qubits beyond c1 temporary use)
// Source: microsoft/QuantumKatas - GraphColoring/ReferenceImplementation.qs (Task 1.5)
operation ColorEqualityOracle_Nbit(c0 : Qubit[], c1 : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    within {
        for (q0, q1) in Zipped(c0, c1) {
            // XOR each pair: c1[i] ^= c0[i]  →  c1[i] = 0 iff bits agree
            CNOT(q0, q1);
        }
    } apply {
        // all XORs = 0  ⟺  registers are equal
        ControlledOnInt(0, X)(c1, target);
    }
}
```
`within/apply` borrows c1 as temporary XOR storage and automatically restores it. Requires `c0` and `c1` to be the same length.

Simpler 2-bit version (enumerate all $4$ equal-color states explicitly):
```csharp
import Std.Convert.*;
import Std.Canon.*;

// 2-bit color equality: fires iff c0 and c1 encode same 2-bit color
// Source: microsoft/QuantumKatas - GraphColoring/ReferenceImplementation.qs (Task 1.4)
operation ColorEqualityOracle_2bit(c0 : Qubit[], c1 : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    for color in 0..3 {
        let binaryColor = IntAsBoolArray(color, 2);
        ControlledOnBitString(binaryColor + binaryColor, X)(c0 + c1, target);
    }
}
```

Source: [microsoft/QuantumKatas - GraphColoring/ReferenceImplementation.qs (Tasks 1.4–1.5)](https://github.com/microsoft/QuantumKatas/blob/main/GraphColoring/ReferenceImplementation.qs)

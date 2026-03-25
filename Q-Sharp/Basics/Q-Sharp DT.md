#Q-Sharp
Q# is **strongly, statically typed** lang. Every value has a type known at compile time. Types divide into classical & quantum categories.

`Int` - $64$-bit signed int. Literals: `42`, `-7`. Used for loop counters, qubit indices, int encoding of bit strings.
`BigInt` - arbitrary-precision int. Literal suffix `L`: `1000000000000L`. Used in Shor's algorithm for modular arithmetic.
`Double` - $64$-bit floating point. Literals: `3.14`, `1.0e-3`. Used for rotation angles, probabilities.
`Bool` - `true` or `false`. Used in conditionals & [[Oracle]] bit patterns.
`String` - immutable string. Used in `Message(...)` for debug output only; not a computation type.
`Result` - quantum measurement outcome: `$0$` or `One`. Returned by `M`, `Measure`, etc. Can be compared: `result == One`.
`Pauli` - `PauliI`, `PauliX`, `PauliY`, `PauliZ`. Enum for specifying measurement basis & [[Rotation gates|Pauli rotation axis]].
`Range` - int range. Literal: `0..n-1`, `0..2..10`, `n-1..-1..0` (reverse). Used in `for` loops & array slices.
`Qubit` - represents single qubit. Cannot be copied, compared, or stored in classical data structures. Can only be passed to operations & funcs. Allocated via `use` or `borrow`. See [[Qubit management]].

`T[]` - array of type `T`. Mutable length fixed at creation. Indexing: `arr[i]`. Slicing: `arr[a..b]`. `Length(arr)` returns element count.
```csharp
let qs = Qubit[5];
let angles = [0.0, PI()/2.0, PI()];
```

`(T1, T2, ...)` - **tuple**. Groups values of different types. Accessed by destructuring:
```csharp
let pair = (3, true);
let (n, flag) = pair;
```

`struct` - named record type (Q# 1.0+):
```csharp
struct Point { X : Double, Y : Double }
let p = Point(1.0, 2.0);
Message($"x = {p.X}");
```

Operations & funcs are **first-class values** in Q# - they can be passed as arguments & returned from other callables. 

Operation type: `(InputType => OutputType)` or with characteristics `(InputType => OutputType is Adj + Ctl)`.
```csharp
operation ApplyGate(op : Qubit => Unit is Adj, q : Qubit) : Unit {
    op(q);
    Adjoint op(q);
}
ApplyGate(H, myQubit);
```

func type: `(InputType -> OutputType)`. No characteristics (funcs are always pure).
```csharp
function Square(x : Int) : Int { return x * x; }
let f : Int -> Int = Square;
```

**`Unit` type**. Equivalent to `void`. Used as return type for operations with no output:
```csharp
operation Reset(q : Qubit) : Unit { ... }
```

`let` declares immutable binding (type inferred):
```csharp
let n = 5; // Int
let theta = PI(); // Double
```

`mutable` declares a mutable variable; use `set` to update:
```csharp
mutable count = 0;
set count += 1;
```

`Std.Convert` provides conversion between types common in quantum algorithms:
```csharp
let bits  = IntAsBoolArray(11, 4); // [true, false, true, true]
let value = ResultArrayAsIntBE(results); // Result[] → Int
let d = IntAsDouble(n); // Int → Double
```

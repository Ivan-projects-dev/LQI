#Q-Sharp
**Copy-and-update** produces new value with one or more fields/elements changed, leaving the original untouched. Uses `w/` (with) & `<-` (assign) syntax. Essential because Q# values are immutable by default.

**Arrays** - update single index or range:
```csharp
let arr = [1, 2, 3, 4];
let arr2 = arr w/ 2 <- 99; // [1, 2, 99, 4] - index 2 replaced
let arr3 = arr w/ 0..1 <- [10, 20]; // [10, 20, 3, 4] - range replaced
Message($"{arr}"); // [1, 2, 3, 4] - original unchanged
```

Slice range on the left must have same length as the replacement array on the right. Range step allowed:
```csharp
let arr4 = arr w/ 0..2..3 <- [10, 30]; // [10, 2, 30, 4] - indices 0, 2 replaced
```

**UDTs & structs** - update named items with `::` accessor on the left:
```csharp
newtype Complex = (Re : Double, Im : Double);
let z = Complex(1.0, -2.0);
let z2 = z w/ Re <- 5.0; // Complex(5.0, -2.0)
let z3 = z w/ Im <- 0.0; // Complex(1.0, 0.0)

// Q# 1.0+ struct syntax
struct Point { X : Double, Y : Double }
let p = Point(1.0, 2.0);
let p2 = p w/ X <- 9.0; // Point(9.0, 2.0)
```

**Mutable update shorthand `w/=`** mutates a `mutable` variable in place. Equivalent to `set x = x w/ ... <- ...`:
```csharp
mutable counts = [0, 0, 0, 0];
set counts w/= 2 <- counts[2] + 1; // increment index 2
mutable z = Complex(0.0, 0.0);
set z w/= Re <- 3.14; // update Re field
```

**Tuples** cannot be updated with `w/` directly - destructor & reconstruct:
```csharp
let t = (1, true, 3.0);
let (a, b, c) = t;
let t2 = (a, false, c); // manual reconstruction
```

**Chaining** - multiple `w/` expressions can be chained (right-associative):
```csharp
let p3 = p w/ X <- 1.0 w/ Y <- 2.0; // Point(1.0, 2.0) from any p
```

**Common pattern: building result arrays** from measurement. Qubit arrays cannot be copied, but `Result[]` can be updated:
```csharp
mutable results = [Zero, size = n];
for i in 0..n-1 {
    set results w/= i <- M(register[i]);
}
```

`[Zero, size = n]` - shorthand for array of `n` copies of `Zero`. Works with any value & type: `[0, size = 10]`, `[false, size = 4]`.

Copy-and-update vs UDT `!` unwrap: `w/` works directly on UDT named items without unwrapping. Only need `!` when passing to  func expecting the underlying tuple type.
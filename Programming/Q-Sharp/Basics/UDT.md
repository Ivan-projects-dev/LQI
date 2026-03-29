#Q-Sharp
**User-Defined Types (UDTs)** are named wrappers over existing types, providing type safety without runtime cost. Declared with `newtype` (Q# $0.x$ syntax; Q# $1.0+$ uses `struct` - see [[Q-Sharp DT]]).

**Anonymous items:**
```csharp
newtype Pair = (Int, Int);
let origin = Pair(0, 0);
```

**Named items:**
```csharp
newtype Complex = (Re : Double, Im : Double);
let z = Complex(1.0, -2.0);
```

**Unwrap operator `!`** - converts UDT back to its underlying tuple type:
```csharp
let originTuple = origin!; // (0, 0) : (Int, Int)
```

**Access named items with `::`**:
```csharp
let real = z::Re; // 1.0
let imag = z::Im; // -2.0
```

**Update-and-reassign named items with `w/=`**:
```csharp
mutable p = Complex(0., 0.);
set p w/= Re <- 1.0; // p is now Complex(1.0, 0.)
```

**[[Copy-and-update]] expression (non-mutating):**
```csharp
let p2 = p w/ Im <- 3.0; // returns new Complex(1.0, 3.0); p unchanged
```

UDTs cannot have methods; they are pure data containers. Use functions or operations that take UDT arguments instead. UDTs are **not** interchangeable with their underlying types without explicit unwrapping - passing `Pair` where `(Int, Int)` is expected requires `myPair!`.

## Sources
- [UDTs & structs in Q#](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/typesystem/udts-and-structs)
- [Q# type system](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/typesystem/)

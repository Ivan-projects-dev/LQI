#Q-Sharp
Q# supports **type-parameterized/generic** callables using type parameters written as `'T`. Single callable body works for any concrete type substituted at the call site.

Type parameters are declared in angle brackets after the callable name:
```csharp
function Identity<'T>(x : 'T) : 'T {
    return x;
}

let n = Identity<Int>(5);
let b = Identity<Bool>(true);
```

Type argument can be inferred from the value, so explicit `<Int>` is usually omitted:
```csharp
let n = Identity(5);        // inferred 'T = Int
```

Operations can be generic too. Type parameter may appear in argument types, return types, or both:
```csharp
operation ApplyTwice<'T>(op : 'T => Unit, x : 'T) : Unit {
    op(x);
    op(x);
}

ApplyTwice(H, myQubit);     // 'T = Qubit
ApplyTwice(X, myQubit);
```

Characteristics (`is Adj`, `is Ctl`) can be attached to generic operation parameters:
```csharp
operation ApplyAndUndo<'T>(op : 'T => Unit is Adj, x : 'T) : Unit {
    op(x);
    Adjoint op(x);
}
```

**Standard library uses of generics**

Most `Std.Arrays` & `Std.Canon` functions are generic. Examples:
```csharp
// Mapped : ('T -> 'U, 'T[]) -> 'U[]
let doubled = Mapped(x -> x * 2, [1, 2, 3]); // [2, 4, 6]

// Filtered : ('T -> Bool, 'T[]) -> 'T[]
let evens = Filtered(x -> x % 2 == 0, [1,2,3,4]); // [2, 4]

// Fold : (('State, 'T) -> 'State, 'State, 'T[]) -> 'State
let sum = Fold((acc, x) -> acc + x, 0, [1,2,3]);// 6
```

`ApplyToEach` works on any qubit operation:
```csharp
// ApplyToEach : ('T => Unit, 'T[]) -> Unit  (simplified)
ApplyToEach(H, register);
ApplyToEach(X, register);
```

**Constraints & limitations**

Q# type parameters are unconstrained - there is no `where 'T : IComparable` syntax. The only constraint is structural: the concrete type must support whatever operations the body calls on `'T`.

Arithmetic operators (`+`, `*`, etc.) are **not** generic in Q#. A function `function Add<'T>(a : 'T, b : 'T) : 'T { return a + b; }` is compile error - there is no num typeclass. Write separate functions for `Int` & `Double`.

Qubit arrays work with generic operations because `Qubit` satisfies the `'T => Unit` call shape, but `Qubit` values themselves cannot be compared, copied, or stored generically.

**Multiple type parameters**: Callable can have $>1$ type parameter:
```csharp
function Pair<'A, 'B>(a : 'A, b : 'B) : ('A, 'B) {
    return (a, b);
}

let p = Pair(3, true);    // (Int, Bool)
```

Q# 1.0+ allows type parameters on structs (record types):
```csharp
struct Box<'T> { Value : 'T }
let b = Box<Int>(Value = 42);
Message($"{b.Value}");
```

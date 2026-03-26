#Q-Sharp
**Bitwise** - operate on `Int` or `BigInt`:
```csharp
0b1010 &&& 0b1100 // 0b1000 = 8 (AND)
0b1010 ||| 0b1100 // 0b1110 = 14 (OR)
0b1010 ^^^ 0b1100 // 0b0110 = 6 (XOR)
~~~0b1010 // bitwise NOT (two's complement for Int)
1 <<< 4 // 16 (left shift - triple chevron, not <<)
16 >>> 2 // 4 (right shift)
```
Triple chevron `<<<`/`>>>` avoids ambiguity with generic type brackets `<'T>`. 

**Ternary-style: conditional expression**
Q# has no `? :` ternary. Use `if` as expression instead:
```csharp
let abs = if x >= 0 { x } else { -x };
let label = if n == 0 { "zero" } elif n > 0 { "positive" } else { "negative" };
```

**Range operators** - produce `Range` values used in `for` loops & array slices:
```csharp
0..n-1 // 0, 1, ..., n-1 (start..end, inclusive)
0..2..10 // 0, 2, 4, 6, 8, 10 (start..step..end)
n-1..-1..0 // n-1, n-2, ..., 0 (reverse)
0... // 0 to end of array (open-ended, only in slices)
```

**String operators**
```csharp
let s = "hello " + "world";         // "hello world"  (concatenation)
let msg = $"n = {n}, pi = {PI()}";  // interpolation: any expression inside {}
```
`Message(s)` is the only output func. No string comparison operators in Q# - strings are for debug output only.

**Tuple destructuring** - not an operator but used like one:
```csharp
let (a, b) = (1, true); // destructure tuple
let (q1, q2) = (Qubit(), Qubit()); // allocate & destructure
```

| Level (high → low) | Operators                             |
| ------------------ | ------------------------------------- |
| Highest            | `()` parentheses, `[]` indexing, `w/` |
| Unary              | `-` (neg), `not`, `~~~`               |
| Exponentiation     | `^`                                   |
| Multiplicative     | `*`, `/`, `%`                         |
| Additive           | `+`, `-`                              |
| Bit shift          | `<<<`, `>>>`                          |
| Bitwise AND        | `&&&`                                 |
| [[Bitwise XOR]]        | `^^^`                                 |
| Bitwise OR         | `\|\|\|`                              |
| Comparison         | `==`, `!=`, `<`, `<=`, `>`, `>=`      |
| Logical AND        | `and`                                 |
| Lowest             | `or`                                  |

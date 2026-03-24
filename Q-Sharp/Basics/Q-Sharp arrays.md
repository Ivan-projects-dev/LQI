#Q-Sharp
**Q# arrays** are immutable fixed-length sequences. `[[Std.Arrays]]` provides a rich set of functional-style operations for transforming them - essential for working with qubit registers & classical bit arrays.
```csharp
let arr = [1, 2, 3, 4, 5];
Length(arr) // 5 - num of elements
Head(arr) // 1 - first element
Tail(arr) // [2, 3, 4, 5] - all but first
Most(arr) // [1, 2, 3, 4] - all but last
Rest(arr) // [2, 3, 4, 5] - alias for Tail
```

**Slicing** - range-based access:
```csharp
arr[1..3] // [2, 3, 4]   - indices 1 to 3
arr[0..2..4] // [1, 3, 5]   - step 2
arr[4..-1..0] // [5, 4, 3, 2, 1] - reversed slice
```

**Reversed** - returns new reversed array:
```csharp
Reversed(arr) // [5, 4, 3, 2, 1]
```

**Array literals & `w/` update syntax**:
```csharp
let a = [0, 1, 2];
let b = a w/ 1 <- 99; // [0, 99, 2] - immutable update at index 1
```

**`Mapped(f, arr)` - transform every element**

Applies func `f` to each element; returns new array:
```csharp
import Std.Arrays.*;
import Std.Convert.*;

let doubles = Mapped(x -> x * 2, [1, 2, 3]); // [2, 4, 6]
let asDoubles = Mapped(IntAsDouble, [1, 2, 3]); // [1.0, 2.0, 3.0]
```

Equivalent to `map` in funcal langs. Used to generate angle arrays for [[Rotation gates]]:
```csharp
let angles = Mapped(k -> 2.0 * PI() / IntAsDouble(1 <<< k), 0..t-1);
```

**`Filtered(pred, arr)` - keep matching elements**

Returns array of elements satisfying predicate `pred`:
```csharp
let evens = Filtered(x -> x % 2 == 0, [1, 2, 3, 4, 5]); // [2, 4]
```

**`Fold(f, init, arr)` - accumulate**

Left fold: applies `f(accumulator, element)` across array, starting from `init`:
```csharp
let sum = Fold((acc, x) -> acc + x, 0, [1, 2, 3, 4]); // 10
let product = Fold((acc, x) -> acc * x, 1, [1, 2, 3, 4]); // 24
```

**`Enumerated(arr)` - index-value pairs**

Returns array of `(Int, T)` tuples, pairing each element with its index. Used constantly in [[Oracle]] construction:
```csharp
for (i, q) in Enumerated(register) {
    Controlled R1([control[i]], (2.0 * PI() / IntAsDouble(1 <<< (i+1)), q));
}
```

**`Zipped(a, b)` - pair $2$ arrays**. Returns array of `(T1, T2)` tuples from $2$ equal-length arrays:
```csharp
let pairs = Zipped([1, 2, 3], [true, false, true]);
// [(1, true), (2, false), (3, true)]
```

**`Sorted(cmp, arr)` - sort with comparator**. Returns sorted array. Comparator returns `Int`: negative, $0$, or positive:
```csharp
let sorted = Sorted((a, b) -> a - b, [3, 1, 4, 1, 5]);    // [1, 1, 3, 4, 5]
```

**`Any(pred, arr)` / `All(pred, arr)` / `Count(pred, arr**
```csharp
Any(x -> x > 3, [1, 2, 3, 4]); // true
All(x -> x > 0, [1, 2, 3, 4]); // true
Count(x -> x % 2 == 0, [1,2,3,4]); // 2
```

**`Padded(n, fill, arr)` - extend to length `n`**. Pads array to length `n` with `fill` value (prepends if `n` positive, appends if negative):
```csharp
Padded(5, 0, [1, 2, 3]); // [0, 0, 1, 2, 3]
Padded(-5, 0, [1, 2, 3]); // [1, 2, 3, 0, 0]
```
Used when bit strings need alignment to a fixed width.

**`Flattened(arr)` - flatten nested array**
```csharp
Flattened([[1, 2], [3, 4], [5]]); // [1, 2, 3, 4, 5]
```

**`IndexOf(pred, arr)` - find first matching index**. Returns index of first element satisfying predicate, or `-1`:
```csharp
IndexOf(x -> x == 3, [1, 2, 3, 4]); // 2
```

Arrays are immutable by default. Use `mutable` + `set w/=` for element-wise mutation:
```csharp
mutable results = [Zero, Zero, Zero];
set results w/= 1 <- One; // [Zero, One, Zero]
```

Or append with `+`:
```csharp
mutable log = [];
set log += [result]; // append single element
```
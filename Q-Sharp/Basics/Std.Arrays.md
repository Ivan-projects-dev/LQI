#Q-Sharp
**`Std.Arrays`** is Q#'s standard array library. Import with `import Std.Arrays.*;`. Covers transformation, filtering, search, sort & combining of arrays. All funcs are pure classical - no quantum ops.

**Creation**
```csharp
let rep = Repeated(0, 5); // [0, 0, 0, 0, 0]
let range = RangeAsIntArray(0..4); // [0, 1, 2, 3, 4]
let sized = [false, size = n]; // n copies of false (built-in syntax, no import needed)
```

**Transformation**
```csharp
// Mapped : ('T -> 'U, 'T[]) -> 'U[]
let doubled = Mapped(x -> x * 2, [1, 2, 3]); // [2, 4, 6]
let strings = Mapped(i -> $"q{i}", RangeAsIntArray(0..n-1));

// MappedByIndex : ((Int, 'T) -> 'U, 'T[]) -> 'U[]
let indexed = MappedByIndex((i, x) -> i * x, [1, 2, 3]); // [0, 2, 6]

// MappedOverRange : (Int -> 'T, Range) -> 'T[]
let squares = MappedOverRange(i -> i * i, 0..4); // [0, 1, 4, 9, 16]
```

**Filtering & searching**
```csharp
// Filtered : ('T -> Bool, 'T[]) -> 'T[]
let evens = Filtered(x -> x % 2 == 0, [1,2,3,4,5]); // [2, 4]

// Where : ('T -> Bool, 'T[]) -> Int[]  - returns indices, not values
let evenIdxs = Where(x -> x % 2 == 0, [1,2,3,4]); // [1, 3]

// IndexOf : ('T -> Bool, 'T[]) -> Int  - first matching index, -1 if none
let first = IndexOf(x -> x > 3, [1, 2, 4, 5]); // 2

// Any : ('T -> Bool, 'T[]) -> Bool
let hasNeg = Any(x -> x < 0, [-1, 2, 3]); // true

// All : ('T -> Bool, 'T[]) -> Bool
let allPos = All(x -> x > 0, [1, 2, 3]); // true

// Count : ('T -> Bool, 'T[]) -> Int
let numEvens = Count(x -> x % 2 == 0, [1,2,3,4]); // 2
```

**Folding & accumulation**
```csharp
// Fold : (('State, 'T) -> 'State, 'State, 'T[]) -> 'State
let sum = Fold((acc, x) -> acc + x, 0, [1,2,3,4]); // 10
let prod = Fold((acc, x) -> acc * x, 1, [1,2,3,4]); // 24
let max = Fold((a, b) -> a > b ? a | b, arr[0], arr); // max element
```

**Slicing & restructuring**
```csharp
let arr = [0, 1, 2, 3, 4];
let h = Head(arr); // 0 (first element)
let t = Tail(arr); // 4 (last element)
let r = Rest(arr); // [1, 2, 3, 4] (all except first)
let m = Most(arr); // [0, 1, 2, 3] (all except last)

// Subarray : (Int[], 'T[]) -> 'T[]  - select by index list
let sub = Subarray([0, 2, 4], arr); // [0, 2, 4]

// Chunks : (Int, 'T[]) -> 'T[][]  - split into chunks of given size
let chunks = Chunks(2, [1,2,3,4,5,6]); // [[1,2],[3,4],[5,6]]

// Flattened : 'T[][] -> 'T[]
let flat = Flattened([[1,2],[3,4]]); // [1,2,3,4]
```

**Combining arrays**
```csharp
// Zipped : ('T[], 'U[]) -> ('T, 'U)[]  - pair elements
let pairs = Zipped([1,2,3], ["a","b","c"]); // [(1,"a"),(2,"b"),(3,"c")]

// Unzipped : ('T, 'U)[] -> ('T[], 'U[])  - inverse of Zipped
let (ints, strs) = Unzipped(pairs);

// Interleaved : ('T[], 'T[]) -> 'T[]  - alternate elements
let alt = Interleaved([1,3,5], [2,4,6]); // [1,2,3,4,5,6]

// Windows : (Int, 'T[]) -> 'T[][]  - sliding window
let wins = Windows(3, [1,2,3,4,5]); // [[1,2,3],[2,3,4],[3,4,5]]
```

**Sorting & permutation**
```csharp
// Sorted : (('T, 'T) -> Bool, 'T[]) -> 'T[]  - stable sort
let asc  = Sorted((a, b) -> a <= b, [3,1,4,1,5]); // [1,1,3,4,5]
let desc = Sorted((a, b) -> a >= b, [3,1,4,1,5]); // [5,4,3,1,1]

// Reversed : 'T[] -> 'T[]
let rev = Reversed([1,2,3,4]); // [4,3,2,1]

// Transposed : 'T[][] -> 'T[][]  - matrix transpose
let mat = [[1,2],[3,4],[5,6]];
let t = Transposed(mat); // [[1,3,5],[2,4,6]]

// Shuffled : 'T[] -> 'T[]  - random permutation (uses runtime RNG)
let shuffled = Shuffled([1,2,3,4,5]);
```

**Index utilities**
```csharp
// IndexRange : 'T[] -> Range  - equivalent to 0..Length(arr)-1
for i in IndexRange(arr) { ... } // idiomatic loop over indices

// Enumerated : 'T[] -> (Int, 'T)[] - pairs each element with its index
for (i, v) in Enumerated(arr) {
    Message($"arr[{i}] = {v}");
}
```
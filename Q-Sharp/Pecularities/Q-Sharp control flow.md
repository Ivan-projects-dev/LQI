#Q-Sharp
**Q# control flow** structures closely resemble classical langs, with one critical addition: `repeat-until`, designed for probabilistic quantum subroutines.
```csharp
if M(q) == One {
    X(q);          // reset to |0⟩ if measured One
} elif count > 5 {
    Message("Too many attempts");
} else {
    H(q);
}
```
Conditionals on `Result` values are allowed but subject to hardware-specific constraints (mid-circuit measurement support required).

```csharp
for i in 0..n-1 {
    H(register[i]);
}

for q in register {
    X(q);
}

for i in n-1..-1..0 {       // reverse: n-1 down to 0
    T(register[i]);
}
```

```csharp
mutable i = 0;
while i < 10 {
    set i += 1;
}
```

**`repeat-until` (RUS - Repeat Until Success)**

Quantum-specific loop. Repeats a probabilistic circuit until success condition is met. Essential for non-deterministic state preparation & some gate synthesis techs:
```csharp
repeat {
    H(q);
    T(q);
    let result = M(q);
} 
until result == Zero
fixup {
    Reset(q); // clean up before retrying
}
```
`fixup` block runs between iterations when the condition is not yet met. `until` condition is evaluated after each body execution. Unlike classical loops, `repeat-until` can be used inside adjointable operations **if** the body is adjointable - Q# handles the statistical reversal.

Common use: preparing a specific [[Quantum state]] with high probability per shot; or [[Grover search]] with unknown $M$ (retry until valid solution found).
```csharp
function FindFirst(arr : Int[], pred : Int -> Bool) : Int {
    for x in arr {
        if pred(x) { return x; }
    }
    return -1;
}
```

```csharp
if n <= 0 { fail "n must be positive"; }
```

**Restrictions on adjointable operations**

Operations declared `is Adj` must have a body the compiler can invert. This restricts control flow inside them:
- `for` loops: allowed (compiler reverses iteration order & adjoints each gate).
- `if` with classical condition: allowed.
- `if` with `Result` condition (mid-circuit measurement): **not** allowed in `Adj` context.
- `while`: not allowed.
- `repeat-until`: allowed under specific conditions.
- `fail`: allowed (propagates through adjoint).

This is why [[Oracle]] operations always use `within/apply` (see [[Within-Apply pattern]]) rather than manual adjoint [[Logic]] - the compiler handles reversal automatically.

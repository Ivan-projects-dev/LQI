#Q-Sharp
Q# uses **compiler attributes** (annotations prefixed with `@`) to mark entry points, tests, & configure compiler behavior. Attributes appear on the line immediately before the callable they annotate.

**`@EntryPoint`** marks the single operation that the runtime executes when the program runs. Every executable Q# program must have exactly $1$ `@EntryPoint`:
```csharp
@EntryPoint()
operation Main() : Unit {
    use q = Qubit();
    H(q);
    let r = MResetZ(q);
    Message($"Result: {r}");
}
```
Entry point must return `Unit` & take no qubit arguments ([[Qubits]] are allocated inside, not passed from outside). Project with $0$ or multiple `@EntryPoint` annotations is a compile error.

**`@Test`** marks operation as unit test. The Q# test runner discovers & executes all `@Test`-annotated operations automatically via `dotnet test`:
```csharp
@Test()
operation TestBellState() : Unit {
    use (q1, q2) = (Qubit(), Qubit());
    H(q1);
    CNOT(q1, q2);
    // assert both in same basis state
    Fact(MResetZ(q1) == MResetZ(q2), "Bell state mismatch");
    Reset(q2);
}
```
`@Test` operations must return `Unit`. `Fact` from `Std.Diagnostics` serves as the assertion primitive - failure throws, causing the test to fail. Tests run on the default simulator unless a target is specified.

**`@Config`** conditionally includes or excludes code based on the active configuration profile. Used to write operations that behave differently in simulation vs. resource estimation vs. hardware:
```csharp
@Config(Base)
operation PrepareState(q : Qubit) : Unit {
    H(q);   // base configuration: use Hadamard
}

@Config(Unrestricted)
operation PrepareState(q : Qubit) : Unit {
    H(q);
    T(q);   // unrestricted: additional non-Clifford gate
}
```
Valid profile names: `Base` (Clifford+T only), `Unrestricted` (full gate set), `AdaptiveRI`. The compiler includes only the variant matching the active profile, allowing the same codebase to compile cleanly for different hardware targets.

**`@Obsolete`** marks callable as deprecated. Compiler emits a warning whenever the annotated callable is used, with the message provided as argument:
```csharp
@Obsolete("Use MResetZ instead of ManualReset.")
operation ManualReset(q : Qubit) : Unit {
    if M(q) == One { X(q); }
}
```
Callable still compiles & runs - `@Obsolete` is advisory only. Used when evolving library while preserving backward compatibility.

**`@SimulatableIntrinsic`** declares that operation is simulator intrinsic - it has no Q# body but the simulator provides built-in implementation. Used when authoring custom simulators or extending the STL:
```csharp
@SimulatableIntrinsic()
operation MyCustomGate(q : Qubit) : Unit is Adj + Ctl;
```
Not used in ordinary app code; relevant for simulator authors.

Attributes take `()` even when there are no arguments. Multiple attributes stack vertically:
```csharp
@Config(Base)
@Test()
operation MyTest() : Unit { ... }
```
Attributes cannot be applied to `function` callables that are purely classical - `@EntryPoint` & `@Test` only work on `operation`.

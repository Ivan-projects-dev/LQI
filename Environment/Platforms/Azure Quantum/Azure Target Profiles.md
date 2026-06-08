#SoftDev #Q-Sharp 
Azure Quantum compiles Q# programs to QIR (Quantum Intermediate Representation) before submission. Target profiles tell the compiler which QIR features the target device supports. Code that runs under `Unrestricted` can silently fail to compile for `Base` hardware.

**`Unrestricted`** - any Q# program runs. No feature restrictions. Simulators only - no actual Azure hardware targets use this profile. This is the default when running locally in VS Code.

**`Base`** - hardware profile for IonQ & Rigetti through Azure. Key restriction: **`Result` type does not support equality comparison.** No mid-circuit measurement. No classical control on measurement outcomes. All measurements happen at the end.
```csharp
// FAILS on Base - cannot branch on Result
if M(q) == Zero { X(q); }

// OK on Base - measurement only at end
let r = M(q);
```

**`Adaptive RI`** - Quantinuum (H2 emulators + QPUs). Adds mid-circuit measurement + conditional gates. You can compare `Result` in `if` conditions, but the conditional block cannot contain `return` or `set` statements.
```csharp
@EntryPoint(Adaptive_RI)
operation SetToZero(q : Qubit) : Unit {
    if M(q) == One { X(q); }  // mid-circuit measurement + conditional gate
}
```
`Adaptive RI` is required for QPE with iterative measurement, error correction gadgets, and any algorithm that branches on intermediate measurement outcomes.

**`Adaptive RIF`** - adds floating-point operations on top of `Adaptive RI`. No Azure hardware targets yet - local simulator only.
### Hardware Mapping
| Profile       | IonQ | Rigetti | Quantinuum |
| ------------- | ---- | ------- | ---------- |
| `Base`        | Yes  | Yes     | No         |
| `Adaptive RI` | No   | No      | Yes (H2)   |
### Setting the Profile
Compiler auto-selects the most restrictive profile that works for the chosen target. Override manually:
```json
// qsharp.json (project-level)
{ "targetProfile": "adaptive_ri" }
```
```csharp
// Per-file: annotation before entrypoint
@EntryPoint(Adaptive_RI)
operation Main() : Unit { ... }
```
```python
# Python host
from qdk import init, TargetProfile
init(target_profile=TargetProfile.Adaptive_RI)
```
### Common Failure Pattern
Write code under `Unrestricted` (local sim), deploy to IonQ (`Base`), get compile error. Fix is always the same: remove all `if M(q) == ...` branching from the circuit body & move to post-processing. If mid-circuit measurement is required, switch to Quantinuum (`Adaptive RI`).
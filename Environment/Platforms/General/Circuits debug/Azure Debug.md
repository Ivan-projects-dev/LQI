#SoftDev #Q-Sharp 
Q# debugging is different from circuit-based platforms. There is no circuit object to inspect, no `draw()`, no `get_counts()`. The debugging tools are `DumpMachine`, `Message`, & the simulator's assertion funcs.
### DumpMachine - The Only Way to See State
`DumpMachine()` prints the full statevector during simulation. It has no hardware equivalent - it only works when running against a simulator. Insert it between every gate pair when debugging new circuits.
```qsharp
operation DebugBell() : Unit {
    use (q0, q1) = (Qubit(), Qubit());
    H(q0);
    DumpMachine();   // prints state after H: (|00⟩ + |10⟩)/√2
    CNOT(q0, q1);
    DumpMachine();   // prints state after CNOT: (|00⟩ + |11⟩)/√2
    ResetAll([q0, q1]);
}
```
Output shows each basis state, its amplitude, probability, & phase. A state with unexpected nonzero amplitude means a gate is wrong or missing.
### Reading DumpMachine Output
DumpMachine prints one row per basis state with nonzero amplitude. Reading it:
- Amplitude column: complex num - magnitude is probability, angle is phase
- Probability column: $|\text{amplitude}|^2$ - what you'd see in measurement statistics
- A state you don't expect with nonzero probability means a gate is wrong, missing, or applied to the wrong qubit

Expected output after `H(q0)` on $2$ [[Qubits]]:
```
# |Basis| Amplitude      | Probability | Phase
# |00⟩  | 0.707+0.000i   | 50.0000%    | 0.0000
# |10⟩  | 0.707+0.000i   | 50.0000%    | 0.0000
```
If `|01⟩` or `|11⟩` appear, the H gate was applied to the wrong qubit index.
### Verifying With Assertions
Use `AssertMeasurementProbability` to check expected probabilities without collapsing state:
```qsharp
AssertMeasurementProbability(
    [PauliZ], [q], Zero, 0.5,   // expect 50% chance of measuring 0
    "Expected superposition", 1e-5);
```
If the assertion fails, the simulator throws immediately with the message - pinpoints exactly which gate broke the invariant.
### Message for Classical Debugging
`Message($"value: {myVar}")` prints to output during simulation. Use it to trace classical control flow when the quantum part looks correct but the overall algorithm produces wrong results.

Source: [Debugging Q# programs — Azure Quantum MS Learn](https://learn.microsoft.com/en-us/azure/quantum/how-to-work-with-jobs)

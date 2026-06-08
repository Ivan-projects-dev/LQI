#SoftDev #Q-Sharp 
**[[QRE]] is free & instant.** No Azure account needed. Run it at `quantum.microsoft.com` or in VS Code. It gives concrete qubit & runtime estimates for any algorithm in seconds. Use it before writing a single line of hardware code.

**[[Quantum Katas]] work in the browser.** No local setup. Each task has auto-checked assertions - your solution passes only when mathematically correct, not just when it runs.

**Local Q# simulation is instant.** No cloud connection, no credentials. The QDK local simulator runs operations up to $\sim 25$–$30$ [[Qubits]] with no overhead.

There are no print statements, no breakpoints, no variable watches. The debugger is `DumpMachine()` - it prints the full statevector during simulation. It has no hardware equivalent. Insert it between every gate pair when debugging new circuits.

**`DumpMachine()` output goes to Debug Console, not Terminal.** In VS Code: `Ctrl+Shift+Y`. If you don't see output, you're looking in the wrong place.

**Reset is mandatory.** Forgetting `Reset(q)` or `ResetAll([...])` before [[Qubits]] leave scope causes a runtime error on the simulator. More insidious: on repeated simulation runs, [[Qubits]] that weren't reset start in non-$|0⟩$ states & produce silently wrong results. Always use `MResetZ` (measures & resets in one step) or `ResetAll` at the end of every operation.

**Target profile breaks code that works in simulation.** `Unrestricted` (local simulator) allows anything. `Base` (hardware) allows only gates & terminal measurement - no classical control within shot. `Adaptive RI` allows mid-circuit measurement & conditional gates. Code that uses measurement-based branching will compile in simulation & fail when targeting hardware without `@Config(AdaptiveRI)`.

**$3$ credit currencies that don't convert.** AQC ([[IonQ]]/[[Rigetti]]), AQT ([[IonQ]] native), HQT ([[Quantinuum]]) are independent. Exhausting one does not affect the others. Budget per provider separately.

**Language server startup.** QDK takes $60+$ seconds on first VS Code launch - it compiles the Q# compiler in the background. Wait for the status bar to stop showing "Q# Loading" before running anything. Subsequent launches are faster.

**[[Azure Workspace]] friction for hardware.** Unlike IBM (no credit card, sign up & run), Azure requires an Azure account plus a quantum workspace before you can submit a single job to hardware. More setup than any other platform. Worthwhile for [[Quantinuum]] (all-to-all connectivity, best error rates) or resource estimation - not for first QPU experiments.

**[[Quantinuum]] is the most expensive option.** HQT credits deplete quickly on non-trivial circuits. Use the free H1 emulator (accessible without Azure subscription via `quantum.microsoft.com`) for dev. Only submit to hardware when the circuit is verified & the experiment is intentional.

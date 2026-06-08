#SoftDev 
How to actually figure out what is wrong with quantum circuit. Per-platform techniques.

Core problem - you cannot watch quantum circuit run as measurement destroys the state. If you add measurement mid-circuit to check intermediate state, you collapse superposition & change the computation. This makes quantum debugging fundamentally different from classical debugging.

Strategy - **reconstruct the state from statistics, not direct observation.**
1. **Test on simulator first.** If it fails on simulator, it is circuit logic bug. If it passes simulator but fails on hardware, it is noise/transpilation issue. Never skip this split.
2. **Use known circuits as sanity checks.** Before debugging your own circuit, run Bell state. If Bell state gives wrong results, your env is broken - not your circuit.
3. **Compare shot counts, not single shots.** One weird outcome proves nothing. Patterns in $1000$+ shots reveal the truth.
4. **Reduce circuit depth.** Remove gates one at time until the circuit starts working. Last removed gate is the problem.

[[PennyLane Debug]]
[[IBM Debug]]
[[Amazon Debug]]
[[Azure Debug]]
[[D-Wave Debug]]


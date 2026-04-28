#SoftDev 
How to actually figure out what is wrong with quantum circuit. Per-platform techniques.

Core problem - you cannot watch quantum circuit run as measurement destroys the state. If you add measurement mid-circuit to check intermediate state, you collapse superposition & change the computation. This makes quantum debugging fundamentally different from classical debugging.

Strategy - **reconstruct the state from statistics, not direct observation.**
1. **Test on simulator first.** If it fails on simulator, it is circuit [[Logic]] bug. If it passes simulator but fails on hardware, it is noise/transpilation issue. Never skip this split.
2. **Use known circuits as sanity checks.** Before debugging your own circuit, run Bell state. If Bell state gives wrong results, your environment is broken - not your circuit.
3. **Compare shot counts, not single shots.** One weird outcome proves nothing. Patterns in $1000$+ shots reveal the truth.
4. **Reduce circuit depth.** Remove gates one at time until the circuit starts working. Last removed gate is the problem.

[[Circuits debug in PennyLane]]
[[Circuits debug in IBM]]
[[Circuits debug in Amazon]]
[[Circuits debug in D-Wave]]
###  Sources
- [Qiskit: debugging circuits](https://docs.quantum.ibm.com/guides/debug-qiskit)
- [Q# DumpMachine documentation](https://learn.microsoft.com/en-us/qsharp/api/qsharp-core/microsoft.quantum.diagnostics/dumpmachine)
- [PennyLane: inspecting circuits](https://docs.pennylane.ai/en/stable/introduction/inspecting_circuits.html)
- [Amazon Braket: local simulators](https://docs.aws.amazon.com/braket/latest/developerguide/braket-devices.html)
- [D-Wave: chain breaks](https://docs.dwavesys.com/docs/latest/c_qpu_annealing.html)

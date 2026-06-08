#SoftDev 
Free resources on [[PennyLane]]/[[Xanadu]], [[Amazon Braket]], & [[D-Wave]] Leap - what each gives you & how to use it.
### Xanadu
Everything core to [[PennyLane]] is free & runs locally. There is no free cloud QPU tier, but the simulation tools are the best available for differentiable quantum circuits.

| Resource                  | Cost                     | What you get                                   |
| ------------------------- | ------------------------ | ---------------------------------------------- |
| `default.qubit` simulator | Free (local)             | Exact statevector, any size up to $~25$ [[Qubits]] |
| `lightning.qubit`         | Free (local)             | C++ accelerated, $~5×$ faster than default     |
| `lightning.gpu`           | Free (local, needs CUDA) | GPU-accelerated simulation                     |
| [[PennyLane]] demos           | Free (browser)           | $100+$ worked algorithm examples               |
| [[PennyLane]] Codebook        | Free (browser)           | Interactive learning modules                   |
| Catalyst / JIT            | Free (local)             | `@qml.qjit` compiler for fast circuits         |
### PennyLane Demos & Codebook
- **Demos:** [pennylane.ai/qml/demonstrations](https://pennylane.ai/qml/demonstrations) - $100+$ notebooks covering [[VQE]], [[QAOA]], QML, [[Error Mitigation]], chemistry
- **Codebook:** [codebook.xanadu.ai](https://codebook.xanadu.ai) - interactive exercises similar to [[IBM Quantum]] Katas, browser-based
Both are free, no account required for reading.
## Amazon Braket

| Resource              | Cost                       | Limit                             |
| --------------------- | -------------------------- | --------------------------------- |
| `LocalSimulator`      | Free                       | Unlimited, runs in process        |
| Free trial credits    | Free (one-time)            | Small credit for new AWS accounts |
| SV1 managed simulator | Paid (per task + per shot) | Charged after free credits        |
| QPU time              | Paid (per task + per shot) | Charged after free credits        |
## D-Wave Leap

| Resource                     | Cost               | Limit                                 |
| ---------------------------- | ------------------ | ------------------------------------- |
| QPU access                   | Free               | **1 minute/month** of QPU solver time |
| Leap Hybrid Solver           | Per-second pricing | No free tier (paid per use)           |
| `SimulatedAnnealingSampler`  | Free (local)       | Unlimited, CPU-based                  |
| Ocean SDK                    | Free               | Full local SDK                        |
| [[D-Wave]] demos & tutorials | Free               | Browser-based                         |

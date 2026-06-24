#SoftDev #Q-Sharp #Python 
**job** is the unit of work in [[Azure Quantum]]. When you submit quantum program to provider target, [[Azure Quantum]] creates job, queues it, executes it on the hardware or simulator, & returns results - all managed by the platform.

**Job lifecycle:** Created $→$ Waiting (in provider queue) $→$ Executing $→$ Succeeded/Failed/Cancelled. You can monitor status & fetch results in the [[Azure Quantum]] portal or via the SDK (`job.get_results()`).

**Input formats** accepted depend on the target: Q# programs, Qiskit circuits, Cirq circuits, or provider-native JSON (e.g., [[IonQ]] JSON, [[OpenQASM]] for [[Quantinuum]]).

**Shots** - each job is submitted with num of shots (repeated circuit executions) to gather measurement statistics. $>$ shots improve result accuracy but increase cost.

**Sessions** group multiple jobs to single target, giving them priority in queue & enabling iterative hybrid algorithms (like [[VQE]] or [[QAOA]]) where classical computation runs between quantum jobs. See [[Azure Sessions]].

Job cost is estimated before submission & depends on the [[Azure Providers|provider's]] billing model (per gate-shot, per HQC, or per exec second).

| Input Format             | Used by                          |
| ------------------------ | -------------------------------- |
| Q# (compiled to QIR)     | All providers via QDK            |
| Qiskit (v1 & v2)         | All providers via QDK transpiler |
| Cirq                     | All providers via QDK            |
| [[OpenQASM]] 2.0 / 3.0   | [[Quantinuum]], QDK              |
| [[IonQ]] JSON            | [[IonQ]] native                  |
| Quil ([[Rigetti]]`.quil.v1`) | [[Rigetti]]                      |
| Pulser                   | [[PASQAL]]                       |
| QIR (`qir.v1`)           | Universal - all targets          |

**QIR (Quantum Intermediate Representation)** is LLVM-based IR that acts as universal compilation target. QDK compiles Q#, Qiskit, & Cirq to QIR before submission.
- **Base profile** - gate + measurement only, no classical branching within circuit. Supported by all providers.
- **Adaptive RI profile** - adds mid-circuit measurement, real-time classical compute, qubit reuse, & conditional gates within a single shot. Required for integrated hybrid. Currently supported on select [[Quantinuum]] & [[IonQ]] targets.

Job Management
```python
target = workspace.get_targets("ionq.qpu.aria-1")
job = target.submit(circuit, shots=1000, name="my-job")
job.wait_until_completed()
results = job.get_results() # returns dict of bitstring -> count
```
Cancel a queued job: `job.cancel()`. Jobs in `Executing` state cannot be cancelled. Cost estimate is displayed via `target.estimate_cost(circuit, shots=N)` before submission.

Source: [Job management — Azure Quantum MS Learn](https://learn.microsoft.com/en-us/azure/quantum/how-to-work-with-jobs)

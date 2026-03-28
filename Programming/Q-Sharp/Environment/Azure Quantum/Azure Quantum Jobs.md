#Quantum #Cloud #Q-Sharp
**job** is the unit of work in [[Azure Quantum]]. When you submit a quantum program to a provider target, [[Azure Quantum]] creates a job, queues it, executes it on the hardware or simulator, & returns results - all managed by the platform.

**Job lifecycle:** Created $→$ Waiting (in provider queue) $→$ Executing $→$ Succeeded/Failed/Cancelled. You can monitor status & fetch results in the [[Azure Quantum]] portal or via the SDK (`job.get_results()`).

**Input formats** accepted depend on the target: Q# programs, Qiskit circuits, Cirq circuits, or provider-native JSON (e.g., IonQ JSON, OpenQASM for Quantinuum).

**Shots** - each job is submitted with num of shots (repeated circuit executions) to gather measurement statistics. $>$ shots improve result accuracy but increase cost.

**Sessions** group multiple jobs to single target, giving them priority in queue & enabling iterative hybrid algorithms (like [[VQE]] or [[QAOA]]) where classical computation runs between quantum jobs. See [[Azure Quantum Sessions]].

Job cost is estimated before submission & depends on the [[Azure Quantum Providers|provider's]] billing model (per gate-shot, per HQC, or per exec second).

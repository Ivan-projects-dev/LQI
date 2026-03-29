#Quantum #Cloud #HybridComputing
**Session** is logical grouping of multiple [[Azure Quantum Jobs|jobs]] submitted to the same provider target. Jobs inside session receive **priority queue access**, reducing wait times compared to standalone job submissions.

Designed for **hybrid quantum-classical algorithms**, where classical computation must happen between quantum circuit runs - the session keeps the connection to the target "warm" across many iterations. Typical use cases include **[[VQE]]** & **[[QAOA]]**.

Qubit states do **not** persist between jobs within session - each job starts with fresh [[Qubits]]. Advantage is reduced queuing overhead, not persistent quantum memory.

Sessions are supported on all [[Azure Quantum]] hardware providers (IonQ, Quantinuum, Rigetti). Open session using the `azure-quantum` Python SDK:
```python
with target.open_session(max_session_timeout_secs=...) as session:
    job1 = target.submit(circuit_1)
    # process results classically
    job2 = target.submit(circuit_2)
```

Sessions complement **integrated hybrid computing**, where classical & quantum [[Logic]] are interleaved within single compiled Q# program running on hardware that supports mid-circuit measurement.

## Sessions vs. Integrated Hybrid

There are two distinct hybrid computing models in [[Azure Quantum]]:

**Sessions (interactive hybrid)** - multiple separate jobs submitted to the same target with priority queue access. Classical computation happens *between* jobs on the client side. Qubit state does NOT persist between jobs. Suited for outer-loop optimization (e.g., parameter updates in [[VQE]] or [[QAOA]]).

**Integrated hybrid** - requires `Adaptive RI` target profile. Classical [[Logic]] & quantum gates are *interleaved within a single compiled program* running directly on hardware that supports mid-circuit measurement. Enables real-time feedback loops, qubit reuse, & adaptive circuit execution based on measurement outcomes - all without network round-trips between the classical & quantum layers.

## Mid-Circuit Measurement & Qubit Reuse

Targets supporting `Adaptive RI` can:
- Measure a qubit mid-circuit & use the result to classically condition later gates
- Reset & reuse the same physical qubit in the same shot
- Apply real-time classical compute (branching, arithmetic) between gate layers

This is crucial for quantum error correction protocols & active feed-forward circuits.

## Session Timeout & Limits

`max_session_timeout_secs` controls how long the session stays open waiting for new jobs. If exceeded, the session closes & remaining jobs revert to normal queue priority. Sessions are billed the same as standalone jobs - no session overhead fee.

## Sources
- [Get started with sessions](https://learn.microsoft.com/en-us/azure/quantum/hybrid-computing-interactive)
- [Hybrid computing overview](https://learn.microsoft.com/en-us/azure/quantum/hybrid-computing-overview)
- [Integrated hybrid (Adaptive RI)](https://learn.microsoft.com/en-us/azure/quantum/hybrid-computing-integrated)
- [Hybrid computing concepts](https://learn.microsoft.com/en-us/azure/quantum/hybrid-computing-concepts)

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

#SoftDev #Q-Sharp #Python  
**Use the Best Backend for Your Circuit Size**

Submitting $2$-qubit circuit to $127$-qubit machine is wasteful - the queue is longer & the transpilation is $>$ complex. Match circuit size to backend size.

| Circuit [[Qubits]] | Recommended backend type | Why |
|---|---|---|
| $1$-$3$ | $5$-$7$ qubit backend | Shorter queue, better per-qubit calibration |
| $4$-$10$ | $27$ qubit backend | `ibm_nairobi`, `ibm_oslo` |
| $10$+ | $127$ qubit backend | `ibm_brisbane`, `ibm_kyiv` |

```python
service = QiskitRuntimeService()

# Filter by min qubit count & prefer least busy
backends = service.backends(filters=lambda b: b.num_qubits >= 5 & b.num_qubits <= 10 and b.status().operational)
backend = min(backends, key=lambda b: b.status().pending_jobs)
```
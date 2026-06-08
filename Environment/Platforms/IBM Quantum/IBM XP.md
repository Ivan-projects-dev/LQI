#SoftDev #Python #Hardware 
**`execute()` is gone.** Any tutorial pre-$2024$ using `execute(circuit, backend)` is broken. Old API was removed in Qiskit $1.0$. Use `SamplerV2` or `EstimatorV2` for all real hardware & runtime jobs. This may break most Stack Overflow (& sometimes LLM) answers silently.

**Transpilation multiplies circuit depth.** IBM's native gate set is `{ECR, RZ, SX, X}`. $10$-gate circuit compiles to $40+$ native gates.  [[SWAP]] becomes $3$ CNOTs. Always check `tqc.depth()` after `transpile()` & use `optimization_level=3`.

**SamplerV2 result extraction is not `get_counts()`.** Path is:
```python
pub_result = result[0]
counts = pub_result.data.meas.get_counts()  # 'meas' only with measure_all()
```
If you used `qc.measure([0,1],[0,1])` manually, the register is named `'c'` not `'meas'`. Mismatch gives `KeyError`.

**FakeBackends exist & are the right tool.** Before spending QPU minutes, test with realistic noise model locally:
```python
from qiskit_aer import AerSimulator
from qiskit_ibm_runtime.fake_provider import FakeSherbrooke
noisy_sim = AerSimulator.from_backend(FakeSherbrooke())
```
Most people don't know these exist & go straight to the QPU.

**QPU errors are physics, not bugs.** Bell state giving `{'00': 490, '11': 507, '01': 15, '10': 12}` is correct behavior. $27$ wrong answers = noise. If your circuit has depth $> 50$, results on real hardware may be dominated by noise regardless of circuit correctness.

**Queue times.** Real QPU access during peak hours: $30$ min – $3$ hours. `least_busy()` helps but doesn't eliminate waiting. Design workflow around async: submit, close laptop, come back.

**$10$ min/month QPU disappears fast - & the allocation changed.** Base Open Plan gives $10$ min per $28$-day rolling window. As of March $2026$, users who hit $20$ min of usage in any $12$-month period can opt into a one-time promotion of $180$ min for the next $12$ months. Regardless: overhead per job (queue, compilation, result return) often costs more time than the circuit itself. Use FakeBackend heavily; treat QPU time as final validation resource.

**Qubit quality varies.** Not all [[Qubits]] on device have equal error rates. 
`backend.properties()` gives live calibration data. For $2$-qubit experiments, pick the qubit pair with the best [[CNOT]] error. `layout` argument in `transpile()` lets you specify which physical [[Qubits]] to use.

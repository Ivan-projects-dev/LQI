#SoftDev #Python #Hardware 
**Transpilation** converts abstract Quantum circuit into hardware-exec form. It is not optional - IBM hardware only runs specific native gate set (`{ECR, RZ, SX, X}`). What comes out is often unrecognizable.

Every `transpile()` call runs $4$ sequential stages:
1. **Unroll** - expand custom/composite gates into primitives
2. **Translate** - convert all gates to the device's native gate set
3. **Layout + Routing** - map logical [[Qubits]] to physical [[Qubits]], insert [[SWAP]] gates where connectivity requires
4. **Optimization** - merge redundant gates, exploit commutativity, reduce depth
Each [[SWAP]] inserted in stage $3$ becomes $3$ CNOTs in stage $2$. Routing overhead alone can $2$–$3\times$ total gate count on complex circuit.

`layout_method` controls how logical [[Qubits]] are mapped to physical [[Qubits]] before routing:
```python
tqc = transpile(qc, backend, layout_method='noise_adaptive', optimization_level=3)
```

| Value              | Behavior                                                                                                             |
| ------------------ | -------------------------------------------------------------------------------------------------------------------- |
| `'trivial'`        | $1$:$1$ mapping - logical qubit $0$ → physical qubit $0$. Ignores connectivity & noise.                              |
| `'dense'`          | Picks a connected subgraph of the coupling map. Topology-aware but noise-blind.                                      |
| `'noise_adaptive'` | Minimizes expected error using live calibration data (readout error + $2$-qubit gate error). Best for real hardware. |
Layout choice alone can change circuit depth by up to $10\times$.

`routing_method` controls how SWAP gates are inserted when $2$ qubits that must interact are not adjacent:

| Value          | Strategy                                                                             |
| -------------- | ------------------------------------------------------------------------------------ |
| `'basic'`      | Greedy - inserts any SWAP that satisfies the next operation. Fast, often suboptimal. |
| `'lookahead'`  | Search algorithm - minimizes total layers added. Better than basic at moderate cost. |
| `'stochastic'` | Deepest search - finds fewest SWAPs overall. Slowest but most optimal.               |
For noisy hardware, `stochastic` routing + `noise_adaptive` layout is the best combination.
### Reproducibility
Transpilation uses randomized heuristics - running the same circuit twice can give different depths:
```python
tqc = transpile(qc, backend, seed_transpiler=42)
```
**Always set `seed_transpiler` when comparing transpiled circuits or running benchmarks.** Without it, results are not reproducible across runs.
### Overriding Basis Gates
```python
tqc = transpile(qc, backend, basis_gates=['cx', 'u'])
```
Overrides the device's default native gate set. Useful when targeting a specific gate family for research or when profiling translation cost.
### Crosstalk Adaptive Scheduling
Parallel gates on adjacent qubits can corrupt each other's state through crosstalk. The transpiler can schedule gates to avoid simultaneous execution on nearby qubits:
```python
tqc = transpile(qc, backend, scheduling_method='alap')
```
This trades some parallelism for better fidelity on dense circuits.
### Optimization Level Behavior
| Level | Effect                                                                                                                                                                 |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $0$   | Minimal. No gate cancellation. For debugging only. **Sometimes produces deeper circuits than level $1$** because compaction at level $1$ enables later-stage benefits. |
| $1$   | Basic gate cancellation, light depth reduction.                                                                                                                        |
| $2$   | Commutation analysis, layout improvement, moderate routing refinement.                                                                                                 |
| $3$   | Maximum optimization. Higher compile time. Diminishing returns vs level $2$ on large circuits.                                                                         |
**Use level $3$ for all real hardware runs** unless compile time is a bottleneck. Check with:
```python
print(f"Original depth: {qc.depth()}")
print(f"Transpiled depth: {tqc.depth()}")
print(f"Native gate counts: {tqc.count_ops()}")
```

Source: [Qiskit transpiler — documentation](https://docs.quantum.ibm.com/api/qiskit/transpiler)
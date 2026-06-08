#SoftDev #Python 
No local installation. Open `[[[[qBraid]]]].com`, launch [[qBraid]] Lab - browser-based JupyterLab with Qiskit, [[PennyLane]], Cirq, Braket SDK, & $30+$ other SDKs preinstalled. Write & run quantum code immediately without `pip install` or env setup. Free plan covers this fully.

Framework conversion is one call:
```python
from qbraid.transpiler import transpile
cirq_circuit = transpile(qiskit_circuit, "cirq")  # automatic gate mapping
```

**[[qBraid]] is hub, not framework.** You still write Qiskit, [[PennyLane]], or Cirq code - [[qBraid]] provides the env & the routing to hardware. If you expect a new quantum API to learn, there isn't one beyond the unified session interface. Primary value is access breadth & zero-setup env.

**Always inspect transpiler output before submitting.** Automatic gate mapping between frameworks handles common gates correctly, but edge cases (decomposition choices, phase conventions, [[Ancilla]] handling) can introduce subtle errors:
```python
from qbraid.transpiler import transpile
braket_circuit = transpile(pennylane_circuit, "braket")
print(braket_circuit)  # verify before running
```
**Free tier is compute-limited for training loops.** Free plan limits session compute time. [[VQE]] or [[QAOA]] optimization ($200+$ circuit evaluations) will hit the limit. Use local dev for heavy training; use [[qBraid]] for final multi-hardware comparison runs.

[[qBraid]]'s actual value: you have circuit in Qiskit & want to run it on [[IonQ]], [[Rigetti]], [[PASQAL]], & IQM without maintaining $4$ separate SDK setups & accounts. Unified access & automatic transpilation save significant boilerplate. For any single-platform work, your primary platform's native tooling is better.

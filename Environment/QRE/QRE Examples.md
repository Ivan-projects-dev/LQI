 #Q-Sharp #Algorithm 
Concrete examples of running the [[QRE]] on real algorithms. Shows how to invoke it, what output fields mean in practice, & what results to expect.

**VS Code (quickest):** open any `.qs` file, right-click the operation name -> *Estimate Resources*. Results appear in a side panel.

**Python inline:**
```python
import qsharp

result = qsharp.estimate('BVEstimate()')
print(result['physicalQubits'])
print(result['runtime'])
```

**Python with qubit model selection:**
```python
result = qsharp.estimate('MyAlgorithm()', params={
        'qubitParams': {'name': 'qubit_gate_us_e3'}, # trapped-ion model
        'qecScheme':   {'name': 'surface_code'},
        'errorBudget': 0.001  # 0.1% total failure probability
    }
)
```

**Batched (Pareto frontier):** submit multiple parameter sets in one call:
```python
results = qsharp.estimate('GroverSearch()', params=[
        {'qubitParams': {'name': 'qubit_gate_ns_e3'}}, # superconducting
        {'qubitParams': {'name': 'qubit_gate_us_e3'}}, # trapped ion
        {'qubitParams': {'name': 'qubit_gate_ns_e4'}}, # optimistic superconducting
    ]
)
results.summary_data_frame() # DataFrame comparing all configs
```

**Space-Time Pareto Frontier**: sweep $T$-factory count to show the qubit-vs-runtime tradeoff:
```python
results = qsharp.estimate('GroverEstimate()',
    params=[{'constraints': {'maxTFactories': k}} for k in [1, 5, 10, 20, 50, 100]]
)
for r in results:
    print(f"T-factories: {r['tFactories']:4d} | Qubits: {r['physicalQubits']:8,} | Runtime: {r['runtime']}")
```

**Typical pattern:**
```
T-factories:    1 | Qubits:   120,000 | Runtime: 10 minutes
T-factories:   10 | Qubits:   250,000 | Runtime: 1 minute
T-factories:   50 | Qubits:   800,000 | Runtime: 12 seconds
T-factories:  100 | Qubits: 1,500,000 | Runtime: 6 seconds
```

More T-factories run in parallel -> algorithm never waits for $T$-states -> faster but uses $>$ [[Qubits]].

**`tFactoryFraction` $> 0.9$:** T-factories dominate. algorithm is $T$-gate heavy. Reducing $T$-count gives max resource saving.

**`codeDistance` $> 25$:** physical error rate is too high for this circuit depth. Either the hardware is too noisy or the algorithm is too long. Redesign for shallower depth or wait for better hardware.

**`runtime` $> 1$ year:** the algorithm is not runnable on any near-term [[FTQC]] hardware. Applies to [[Shor]] factoring at $RSA-2048$ on current qubit models.

**`physicalQubits` $< 10,000$:** within range of near-term fault-tolerant experiments. Current leading hardware (Google Willow $105$ [[Qubits]], IBM Heron $133$ [[Qubits]]) is approaching this regime for small error-corrected circuits.
###  Sources
- [Resource Estimator quickstart](https://learn.microsoft.com/en-us/azure/quantum/quickstart-microsoft-resources-estimator)
- [Output fields reference](https://learn.microsoft.com/en-us/azure/quantum/overview-resources-estimator)
- [Batched estimation](https://learn.microsoft.com/en-us/azure/quantum/resource-estimator-batching)
- [Qubit parameter models](https://learn.microsoft.com/en-us/azure/quantum/overview-resources-estimator#physical-qubit-parameters)
- [Space-time tradeoff blog](https://quantum.microsoft.com/en-us/insights/blogs/resource-estimation/exploring-space-time-tradeoffs-with-azure-quantum-resource-estimator)
#Q-Sharp #Quantum
Concrete examples of running the [[Quantum resource estimator]] on real algorithms. Shows how to invoke it, what output fields mean in practice, and what results to expect.

## How to Run (All Methods)

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
result = qsharp.estimate(
    'MyAlgorithm()',
    params={
        'qubitParams': {'name': 'qubit_gate_us_e3'},  # trapped-ion model
        'qecScheme':   {'name': 'surface_code'},
        'errorBudget': 0.001                          # 0.1% total failure probability
    }
)
```

**Batched (Pareto frontier):** submit multiple parameter sets in one call:
```python
results = qsharp.estimate(
    'GroverSearch()',
    params=[
        {'qubitParams': {'name': 'qubit_gate_ns_e3'}},  # superconducting
        {'qubitParams': {'name': 'qubit_gate_us_e3'}},  # trapped ion
        {'qubitParams': {'name': 'qubit_gate_ns_e4'}},  # optimistic superconducting
    ]
)
results.summary_data_frame()   # DataFrame comparing all configs
```

---

## Example 1 - Bernstein-Vazirani ($n = 20$)

BV is Clifford-only: one H layer, one CNOT layer, one H layer. Zero T-gates.

```csharp
import Std.Arrays.*;
import Std.Convert.*;

operation BVEstimate() : Unit {
    let n = 20;
    let s = 0b10110100110101011010;
    use (input, anc) = (Qubit[n], Qubit());
    X(anc); H(anc);
    ApplyToEach(H, input);
    let bits = IntAsBoolArray(s, n);
    for i in 0..n-1 { if bits[i] { CNOT(input[i], anc); } }
    ApplyToEach(H, input);
    ResetAll(input + [anc]);
}
```

**Expected output (superconducting `qubit_gate_ns_e3`, surface code):**

| Field | Value |
|---|---|
| `physicalQubits` | ~500-2000 |
| `logicalQubits` | ~22 |
| `tStates` | 0 (Clifford only) |
| `tFactories` | 0 |
| `runtime` | microseconds |
| `codeDistance` | 5-7 |

Zero T-gates means zero T-factory overhead. BV is a baseline test any near-term device can run.

---

## Example 2 - Grover Search ($n = 20$, single solution)

~$\sqrt{2^{20}} \approx 1024$ iterations. Each iteration contains a multi-controlled X (T-ladder cost).

```csharp
import Std.Arrays.*;
import Std.Canon.*;
import Std.Math.*;

operation GroverEstimate() : Unit {
    let n = 20;
    let target = 42;
    let nIter = Round(PI() / 4.0 * Sqrt(IntAsDouble(1 <<< n)));
    use register = Qubit[n];
    ApplyToEach(H, register);
    for _ in 1..nIter {
        use anc = Qubit();
        within { X(anc); H(anc); }
        apply  { ControlledOnInt(target, X)(register, anc); }
        within { ApplyToEach(H, register); ApplyToEach(X, register); }
        apply  { Controlled Z(Most(register), Tail(register)); }
    }
    ResetAll(register);
}
```

**Expected output:**

| Field | Approximate value |
|---|---|
| `physicalQubits` | ~100,000-500,000 |
| `logicalQubits` | ~200-400 |
| `tStates` | ~50,000-500,000 |
| `tFactories` | 10-50 parallel |
| `runtime` | seconds to minutes |
| `codeDistance` | 13-17 |

With 1024 iterations and ~100 T-gates per controlled X, total T-count is ~$10^5$. This is what 'quadratic speedup' costs in physical resources.

---

## Example 3 - QPE on T Gate ($t = 10$ phase bits)

QPE estimates the phase $\varphi = 1/8$ of T's eigenvalue. Requires $\sum_{k=0}^{9} 2^k = 1023$ applications of T.

```csharp
import Std.Canon.*;

operation QPEEstimate() : Unit {
    let t = 10;
    use (phaseReg, eigenstate) = (Qubit[t], Qubit());
    X(eigenstate);
    ApplyToEach(H, phaseReg);
    for k in 0..t-1 {
        let Tk = OperationPow(T, 1 <<< k);
        Controlled Tk([phaseReg[k]], eigenstate);
    }
    Adjoint ApplyQFT(phaseReg);
    ResetAll(phaseReg + [eigenstate]);
}
```

**Expected output:**

| Field | Value |
|---|---|
| `physicalQubits` | ~50,000-200,000 |
| `tStates` | 1023 ($2^{10}-1$) |
| `runtime` | milliseconds to seconds |
| `codeDistance` | 11-15 |

T-count scales as $2^t - 1$. For $t=20$ precision bits: ~$10^6$ T-gates, millions of physical qubits.

---

## Example 4 - Comparing Qubit Models

Same algorithm, three hardware assumptions:

```python
import qsharp

models = {
    'Superconducting (1ns, 0.1%)':  'qubit_gate_ns_e3',
    'Superconducting (1ns, 0.01%)': 'qubit_gate_ns_e4',
    'Trapped ion (1us, 0.1%)':      'qubit_gate_us_e3',
}
for name, model in models.items():
    r = qsharp.estimate('QPEEstimate()', params={'qubitParams': {'name': model}})
    print(f"{name}: {r['physicalQubits']} qubits, {r['runtime']}")
```

Better error rate (`e4` vs `e3`) lowers the required code distance, dramatically cutting qubit count.
Trapped-ion: fewer qubits needed (lower error rate) but slower runtime (gate time 1 us vs 1 ns).

---

## Example 5 - Space-Time Pareto Frontier

Sweep T-factory count to show the qubit-vs-runtime tradeoff:

```python
results = qsharp.estimate(
    'GroverEstimate()',
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

More T-factories run in parallel -> algorithm never waits for T-states -> faster but uses more qubits.

---

## Reading the Output: Diagnostic Rules

**`tFactoryFraction` > 0.9:** T-factories dominate. The algorithm is T-gate heavy. Reducing T-count gives the biggest resource saving.

**`codeDistance` > 25:** physical error rate is too high for this circuit depth. Either the hardware is too noisy or the algorithm is too long. Redesign for shallower depth or wait for better hardware.

**`runtime` > 1 year:** the algorithm is not runnable on any near-term FTQC hardware. Applies to Shor factoring at RSA-2048 on current qubit models.

**`physicalQubits` < 10,000:** within range of near-term fault-tolerant experiments. Current leading hardware (Google Willow 105 qubits, IBM Heron 133 qubits) is approaching this regime for small error-corrected circuits.

## Sources
- [Resource Estimator quickstart](https://learn.microsoft.com/en-us/azure/quantum/quickstart-microsoft-resources-estimator)
- [Output fields reference](https://learn.microsoft.com/en-us/azure/quantum/overview-resources-estimator)
- [Batched estimation](https://learn.microsoft.com/en-us/azure/quantum/resource-estimator-batching)
- [Qubit parameter models](https://learn.microsoft.com/en-us/azure/quantum/overview-resources-estimator#physical-qubit-parameters)
- [Space-time tradeoff blog](https://quantum.microsoft.com/en-us/insights/blogs/resource-estimation/exploring-space-time-tradeoffs-with-azure-quantum-resource-estimator)
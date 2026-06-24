#SoftDev #Q-Sharp **Check Calibration Before Submitting**

Real QPU calibration changes every few hours. Backend with $1\%$ [[CNOT]] error rate this morning may have $3\%$ this afternoon. Check before submitting experiments where error rate matters.
```python
props = backend.properties()

# Average CNOT error across all pairs
cx_errors = []
for gate in props.gates:
    if gate.gate == 'cx':
        for param in gate.parameters:
            if param.name == 'gate_error':
                cx_errors.append(param.value)

avg = sum(cx_errors) / len(cx_errors) if cx_errors else 0
print(f"Average CX error: {avg:.4f}")
print(f"Backend: {backend.name}")
```
If average CX error is $>0.02$ ($2\%$), consider waiting for recalibration or choosing different backend.

Source: [Qiskit documentation](https://docs.quantum.ibm.com/)

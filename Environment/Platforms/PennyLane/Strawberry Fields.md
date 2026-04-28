#SoftDev #Python 
Python library for **continuous-variable (CV) / photonic** quantum computing. Operates on optical modes rather than discrete [[Qubits]]; gates include Displacement, Squeezing, Beamsplitter, & Kerr operations.
```python
import strawberryfields as sf
from strawberryfields import ops

prog = sf.Program(2)
with prog.context as q:
    ops.Sgate(0.54) | q[0]
    ops.BSgate(0.43, 0.1) | (q[0], q[1])
    ops.MeasureFock() | q[0]

eng = sf.Engine("fock", backend_options={"cutoff_dim": 5})
result = eng.run(prog)
```

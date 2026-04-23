#Quantum #Beginner #IBM #Experience
Your first 60 minutes on IBM Quantum, including every friction point and how to get past it.

## Before You Open the Browser

Install the libraries first. Doing this after login wastes QPU time while you debug your environment.

```bash
pip install qiskit qiskit-ibm-runtime qiskit-aer pylatexenc
```

`pylatexenc` is optional but `qc.draw('mpl')` will crash without it. Install it now.

## Account Setup (5 Minutes of Forms)

1. Go to `quantum.cloud.ibm.com`
2. Create an IBM ID — no credit card, no payment info
3. After login, click your name (top-right) → **Manage IBM Quantum account**
4. Copy the **API token** — it is a long string starting with roughly `eyJhbGci...`

Save the token once in Python so you never type it again:

```python
from qiskit_ibm_runtime import QiskitRuntimeService

QiskitRuntimeService.save_account(
    channel="ibm_quantum",
    token="YOUR_TOKEN_HERE",
    overwrite=True
)
```

Run this once in a Jupyter cell or standalone script. The token is saved to `~/.qiskit/qiskit-ibm.json`. After this, every future script just calls `QiskitRuntimeService()` with no arguments.

**Common mistake:** Running `save_account()` every time at the top of your notebooks. You only need it once. If you paste your token into every notebook, you risk accidentally sharing it in a screenshot.

## The Dashboard — What to Actually Look At

The IBM Quantum dashboard shows every available backend. The columns that matter:

- **Qubits**: size of the chip
- **Pending jobs**: queue depth right now
- **CNOT Error**: average 2-qubit gate fidelity — lower is better

Ignore backends with more than 50 pending jobs when you are learning. A $20$-minute wait for a $50$-gate circuit that produces noise-dominated output teaches you nothing.

The `ibm_brisbane` (127-qubit) and `ibm_kyiv` (127-qubit) backends are almost always available. For your first run, filter by `Qubits = 7` — the 7-qubit backends (`ibm_nairobi`, `ibm_oslo`, etc.) have shorter queues and your 2-qubit circuits run faster.

## Writing the Circuit

Start with Bell state. Do not invent your own circuit — you need a known-correct reference to detect hardware errors vs. your own mistakes.

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

print(qc.draw())
```

Expected output:
```
        ┌───┐      ░ ┌─┐   
   q_0: ┤ H ├──■───░─┤M├───
        └───┘┌─┴─┐ ░ └─┘┌─┐
   q_1: ─────┤ X ├─░────┤M├
             └───┘ ░    └─┘
```

If you see this, your Qiskit installation is working. The `QuantumCircuit.draw()` with no arguments uses ASCII — it always works, no dependencies required.

## Simulator First — Always

```python
from qiskit_aer import AerSimulator

sim = AerSimulator()
job = sim.run(qc, shots=1024)
result = job.result()
counts = result.get_counts()
print(counts)
```

Expected: `{'00': ~512, '11': ~512}` — roughly equal. The exact numbers vary each run (statistical noise, not hardware noise).

If you see anything other than `00` and `11` outcomes, you have a circuit bug. Fix it here, in simulation, before touching the QPU.

## Transpilation — The Step Everyone Skips

Real quantum chips do not execute your circuit directly. They have a specific set of **native gates** and a specific **connectivity map** (which qubit can CNOT to which other qubit). Transpilation rewrites your circuit into one the chip can actually run.

```python
from qiskit import transpile
from qiskit_ibm_runtime import QiskitRuntimeService

service = QiskitRuntimeService()
backend = service.least_busy(operational=True, simulator=False)

tqc = transpile(qc, backend=backend, optimization_level=3)
print(f"Original gates: {qc.count_ops()}")
print(f"Transpiled gates: {tqc.count_ops()}")
print(f"Circuit depth: {tqc.depth()}")
```

For the Bell state you should see roughly $3$–$5$ native gates after transpilation. The depth should be $3$–$4$.

**What optimization_level does:**
- `0`: Just maps to native gates. Fast compile, dirtier circuit.
- `1`: Basic optimization. Good default.
- `2`: More aggressive. Adds some compile time.
- `3`: Full optimization including routing heuristics. Best fidelity for simple circuits.

For anything under $20$ qubits, `optimization_level=3` is almost always worth it.

## Submitting to Real Hardware

```python
from qiskit_ibm_runtime import SamplerV2

sampler = SamplerV2(backend)
job = sampler.run([tqc], shots=1024)

print(f"Job ID: {job.job_id()}")
print("Job submitted. Status:", job.status())
```

Write down the job ID. If your Python session crashes before you get results, you can retrieve the job later with:

```python
job = service.job("YOUR_JOB_ID_HERE")
```

## Waiting

You will now wait. Check the IBM Quantum dashboard under **Jobs** to see your position in the queue. Do not spam `job.status()` in a loop — it won't make it go faster, and you may get rate-limited.

A realistic wait for a 2-qubit circuit on a $7$-qubit backend:
- Off-peak (weekend mornings, early UTC): $2$–$10$ minutes
- Peak (weekday afternoons): $20$–$60$ minutes

## Retrieving and Reading Results

```python
result = job.result()
pub_result = result[0]
counts = pub_result.data.meas.get_counts()
print(counts)
```

**Why `result[0]`?** `SamplerV2` accepts a list of circuits. You submitted one circuit, so the result has one entry at index $0$.

**Why `.data.meas`?** The measurement register is named `meas` by default when you use `measure_all()`. If you used `qc.measure(qubit, clbit)` with a custom register, the name will differ.

A typical real hardware result for Bell state:
```
{'00': 490, '11': 507, '01': 15, '10': 12}
```

The `01` and `10` entries — about $2.7\%$ of shots — are physical errors. This is normal and expected on current hardware. It does not mean your circuit is wrong.

## Making Sense of the Noise

To visualize:

```python
from qiskit.visualization import plot_histogram
plot_histogram(counts)
```

You will see a bar chart with `00` and `11` tall, and `01` and `10` small. The small bars are the error floor.

To compare directly with simulation:

```python
sim_job = sim.run(qc, shots=1024)
sim_counts = sim_job.result().get_counts()

from qiskit.visualization import plot_histogram
plot_histogram([sim_counts, counts], legend=['Simulator', 'Hardware'])
```

This plot side-by-side shows you exactly how much the chip deviates from ideal. For a 2-qubit circuit, expect $2$–$5\%$ error. For a $10$-gate circuit, expect $10$–$25\%$. For a $50$-gate circuit, the hardware result may be nearly uniform (pure noise).

## What You Actually Learned

After this session, you have hands-on evidence of:
1. Quantum gates produce superposition and entanglement
2. Measurement collapses to classical outcomes
3. Running many shots gives you a distribution, not a single answer
4. Real hardware introduces errors that grow with circuit complexity
5. Transpilation is mandatory — the circuit you write is not the circuit that runs

These are the concepts all quantum textbooks describe abstractly. You now have numbers.

## Next Steps

- Try 3-qubit GHZ state: `H(0)`, `CNOT(0,1)`, `CNOT(1,2)` — see if 3-qubit correlation holds
- Intentionally add noise: copy your Bell state, add 5 extra `X` gates between existing ones, observe the growing error rate
- Read your backend's calibration data: `backend.properties()` — every gate's current error rate is public

## Sources
- [IBM Quantum Getting Started](https://docs.quantum.ibm.com/start)
- [Hello World tutorial](https://docs.quantum.ibm.com/guides/hello-world)
- [SamplerV2 documentation](https://docs.quantum.ibm.com/api/qiskit-ibm-runtime/sampler-v2)
- [Transpilation overview](https://docs.quantum.ibm.com/guides/transpile)

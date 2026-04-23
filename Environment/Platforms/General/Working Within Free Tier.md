#Quantum #Beginner #Experience #Strategy
How to get the most out of limited free QPU access across IBM Quantum, Azure Quantum, and Amazon Braket.

## The Reality of Free Tier

Free QPU access is intentionally limited. IBM Quantum gives you a shared queue with no guaranteed throughput. Azure Quantum provides $500 in credits that disappear fast on QPU calls. Amazon Braket charges per task ($0.30) and per shot ($0.00035), and free tier gives you only $10 of task credit.

The goal is not to use these platforms as infinite computing resources. The goal is to run targeted experiments that teach you something specific, on circuits you have already verified in simulation.

---

## Principle 1: Never Touch the QPU Without a Simulator Pass

Every QPU job you run should answer a specific question that simulation cannot. If you can answer it in simulation, do not spend QPU time.

Questions only QPU can answer:
- What is the actual error rate on this hardware for this circuit?
- Does error mitigation reduce errors by the expected amount?
- Are the results consistent across different backends?

Questions simulation answers fine:
- Is my circuit logically correct?
- Does the algorithm converge to the right answer?
- Is my transpilation valid?

Establish this discipline early. Beginners who skip simulation waste most of their free credits debugging circuit bugs on the QPU.

---

## Principle 2: Batch Your Jobs

Every QPU job has overhead: queue wait time, calibration, readout. Submitting $10$ circuits in one batch takes roughly the same queue time as $1$ circuit.

**IBM Quantum - submit a list of circuits:**

```python
from qiskit_ibm_runtime import SamplerV2
from qiskit import transpile

circuits_to_run = [tqc_1, tqc_2, tqc_3, tqc_4, tqc_5]
sampler = SamplerV2(backend)
job = sampler.run(circuits_to_run, shots=1024)
result = job.result()

for i, pub_result in enumerate(result):
    counts = pub_result.data.meas.get_counts()
    print(f"Circuit {i}: {counts}")
```

A single job with 5 circuits uses 5x the information for the same wait time.

**Amazon Braket - batch tasks:**

```python
from braket.aws import AwsQuantumTaskBatch

tasks = device.run_batch(circuits, s3_destination_folder=s3_folder, shots=1000)
results = tasks.results()  # waits for all to complete
```

---

## Principle 3: Use Fewer Shots on Real Hardware

Simulation shots are free. QPU shots cost money and time. You do not need $8192$ shots on every circuit.

Rules of thumb:
- For qualitative checks (is this the right distribution shape?): $256$-$512$ shots
- For quantitative error rate measurement: $1024$ shots
- For high-precision estimation or rare event detection: $4096$+ shots

A Bell state at $256$ shots still clearly shows the `00`/`11` pattern. You do not need $4096$ shots to confirm that.

For IBM Quantum free tier: $1024$ shots per circuit is the default and is adequate for most educational experiments.

---

## Principle 4: Use the Best Backend for Your Circuit Size

Submitting a $2$-qubit circuit to a $127$-qubit machine is wasteful — the queue is longer and the transpilation is more complex. Match circuit size to backend size.

| Circuit qubits | Recommended backend type | Why |
|---|---|---|
| $1$-$3$ | $5$-$7$ qubit backend | Shorter queue, better per-qubit calibration |
| $4$-$10$ | $27$ qubit backend | `ibm_nairobi`, `ibm_oslo` |
| $10$+ | $127$ qubit backend | `ibm_brisbane`, `ibm_kyiv` |

```python
service = QiskitRuntimeService()

# Filter by minimum qubit count and prefer least busy
backends = service.backends(
    filters=lambda b: b.num_qubits >= 5 and b.num_qubits <= 10
                      and b.status().operational
)
backend = min(backends, key=lambda b: b.status().pending_jobs)
```

---

## Principle 5: Check Calibration Before Submitting

Real QPU calibration changes every few hours. A backend with $1\%$ CNOT error rate this morning may have $3\%$ this afternoon. Check before submitting experiments where error rate matters.

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

If average CX error is above $0.02$ ($2\%$), consider waiting for recalibration or choosing a different backend.

---

## Principle 6: Save Job IDs Immediately

QPU jobs are asynchronous. You submit, wait, come back later. If your Python session dies, the job is still running.

```python
job = sampler.run([tqc], shots=1024)
job_id = job.job_id()
print("SAVE THIS:", job_id)

# Later, in a new session:
from qiskit_ibm_runtime import QiskitRuntimeService
service = QiskitRuntimeService()
job = service.job(job_id)
result = job.result()
```

Keep a text file of job IDs with descriptions. Do not rely on memory or session state.

---

## Azure Quantum Credits - Making $500 Last

Azure Quantum provides $\$500$ in free credits, but QPU costs add up fast. Typical costs:

- IonQ Aria: $\$0.00220$ per gate shot (expensive — a $20$-gate circuit at $1000$ shots = $\$44$)
- Quantinuum H1: $\$0.00975$ per eHQC (very expensive)
- Rigetti: lower cost, but hardware often unavailable

**Strategy:** Use Azure credits for IonQ only for circuits you cannot run on IBM free tier. Trapped-ion hardware has different error characteristics (longer coherence, all-to-all connectivity) - which makes it worth testing for comparison, but not for daily experimentation.

The Azure [[Quantum resource estimator]] is completely free. Use it as much as you want to understand algorithm requirements.

---

## Amazon Braket Free Tier Math

Free tier: $\$10$ total free trial credit (new accounts). After that, you pay.

- SV1 simulator: $\$0.075$ per task + $\$0.00035$ per shot - inexpensive, good for medium circuits
- Local simulator: completely free, runs in your own compute
- QPU task: $\$0.30$ per task + device-specific shot cost

**Strategy:** Use `LocalSimulator` for all development. Only use QPU for final validation runs. Use SV1 for circuits too large for local simulation ($> 25$ qubits).

```python
from braket.devices import LocalSimulator

# Free - runs locally, no AWS charges
device = LocalSimulator()
task = device.run(circuit, shots=1000)
```

---

## Practical Session Structure

A productive free-tier quantum session:

1. Write circuit locally ($5$ minutes)
2. Test on local/free simulator ($2$ minutes)
3. Fix any bugs found ($variable$)
4. Transpile and check depth ($1$ minute)
5. Check backend calibration ($1$ minute)
6. Batch all planned circuits into one job ($2$ minutes)
7. Submit and record job ID ($1$ minute)
8. Wait (go do something else)
9. Retrieve and analyze results ($10$ minutes)

This structure squeezes maximum information out of one QPU job. Typical output: $3$-$8$ validated circuits, all results saved locally, no wasted runs.

---

## When to Stop Using Free Tier

You have outgrown free tier when:
- Your algorithms consistently require $> 100$ two-qubit gates (noise-dominated on current hardware anyway)
- You need to run parameter sweeps with $> 100$ circuit variations
- You need guaranteed low-queue-time for time-sensitive experiments

At that point, consider IBM Quantum Premium plans or academic access programs (many universities have bulk QPU allocations).

## Sources
- [IBM Quantum pricing and plans](https://quantum.ibm.com/services)
- [Azure Quantum credits overview](https://learn.microsoft.com/en-us/azure/quantum/azure-quantum-credits)
- [Amazon Braket pricing](https://aws.amazon.com/braket/pricing/)
- [Qiskit: choosing a backend](https://docs.quantum.ibm.com/guides/get-started-with-primitives)

#SoftDev 
How to get the most out of limited free QPU access across [[IBM Quantum]], [[Azure Quantum]], & [[Amazon Braket]].

Free QPU access is intentionally limited. [[IBM Quantum]] gives you shared queue with no guaranteed throughput. [[Azure Quantum]] provides $500 in credits that disappear fast on QPU calls. [[Amazon Braket]] charges per task ($0.30) & per shot ($0.00035), & free tier gives you only $10 of task credit. 
Goal is not to use these platforms as infinite computing resources but to run targeted experiments that teach something specific, on circuits already verified in simulation.
1. [[Free Tier Principle 1]]: Never Touch the QPU Without a Simulator Pass
2. [[Free Tier Principle 2]]: Batch Your Jobs
3. [[Free Tier Principle 3]]: Use $<$ Shots on Real Hardware
4. [[Free Tier Principle 4]]: Use the Best Backend for Your Circuit Size
5. [[Free Tier Principle 5]]: Check Calibration Before Submitting
6. [[Free Tier Principle 6]]: Save Job IDs Immediately
### Azure Quantum Credits
[[Azure Quantum]] provides $\$500$ in free credits, but QPU costs add up fast. Typical costs:
- IonQ Aria: $\$0.00220$ per gate shot (expensive - a $20$-gate circuit at $1000$ shots = $\$44$)
- Quantinuum $H_1$: $\$0.00975$ per eHQC (very expensive)
- Rigetti: lower cost, but hardware often unavailable

**Strategy:** Use Azure credits for IonQ only for circuits you cannot run on IBM free tier. Trapped-ion hardware has different error characteristics (longer coherence, all-to-all connectivity) - which makes it worth testing for comparison, but not for daily experimentation.

Azure [[QRE]] is completely free. Use it as much as you want to understand algorithm requirements.

### Amazon Braket Free Tier Math
Free tier: $\$10$ total free trial credit (new accounts). After that, you pay.
- $SV1$ simulator: $\$0.075$ per task + $\$0.00035$ per shot - inexpensive, good for medium circuits
- Local simulator: completely free, runs in your own compute
- QPU task: $\$0.30$ per task + device-specific shot cost

**Strategy:** Use `LocalSimulator` for all dev. Only use QPU for final validation runs. Use SV1 for circuits too large for local simulation ($> 25$ [[Qubits]]).
```python
from braket.devices import LocalSimulator

# Free - runs locally, no AWS charges
device = LocalSimulator()
task = device.run(circuit, shots=1000)
```
### When to Stop Using Free Tier
You have outgrown free tier when:
- Your algorithms consistently require $> 100$ two-qubit gates (noise-dominated on current hardware anyway)
- You need to run parameter sweeps with $> 100$ circuit variations
- You need guaranteed low-queue-time for time-sensitive experiments
At that point, consider [[IBM Quantum]] Premium plans or academic access programs (many universities have bulk QPU allocations).
### Sources
- [IBM Quantum pricing & plans](https://quantum.ibm.com/services)
- [Azure Quantum credits overview](https://learn.microsoft.com/en-us/azure/quantum/azure-quantum-credits)
- [Amazon Braket pricing](https://aws.amazon.com/braket/pricing/)
- [Qiskit: choosing a backend](https://docs.quantum.ibm.com/guides/get-started-with-primitives)

#SoftDev 
How to get the most out of limited free QPU access across [[IBM Quantum]], [[Azure Quantum]], & [[Amazon Braket]].

Free QPU access is intentionally limited. [[IBM Quantum]] gives you shared queue with no guaranteed throughput. [[Azure Quantum]] provides a free credit allowance that disappears fast on QPU calls. [[Amazon Braket]] charges per task & per shot, & the free tier credit covers only a handful of cloud simulator tasks. 
Goal is not to use these platforms as infinite computing resources but to run targeted experiments that teach something specific, on circuits already verified in simulation.
1. [[Free Tier Principle 1]]: Never Touch the QPU Without a Simulator Pass
2. [[Free Tier Principle 2]]: Batch Your Jobs
3. [[Free Tier Principle 3]]: Use $<$ Shots on Real Hardware
4. [[Free Tier Principle 4]]: Use the Best Backend for Your Circuit Size
5. [[Free Tier Principle 5]]: Check Calibration Before Submitting
6. [[Free Tier Principle 6]]: Save Job IDs Immediately
### Azure Quantum Credits
[[Azure Quantum]] provides a free credit allowance for new accounts, but QPU costs add up fast:
- IonQ Aria: paid per gate-shot - expensive for deep circuits at high shot counts
- Quantinuum: paid per Hardware Quantum Credit - tends to be the most expensive option
- Rigetti: lower cost per run, but hardware is sometimes unavailable

**Strategy:** Use Azure credits for IonQ only for circuits you cannot run on IBM free tier. Trapped-ion hardware has different error characteristics (longer coherence, all-to-all connectivity) - which makes it worth testing for comparison, but not for daily experimentation.

Azure [[QRE]] is completely free. Use it as much as you want to understand algorithm requirements.

### Amazon Braket Free Tier
Free tier: small one-time trial credit for new accounts. After that, you pay.
- $SV1$ simulator: paid per task + per shot - inexpensive, good for medium circuits
- Local simulator: completely free, runs in your own compute
- QPU task: paid per task + device-specific shot cost

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

Source: [IBM Quantum Platform](https://quantum.cloud.ibm.com/) [Amazon Braket pricing](https://aws.amazon.com/braket/pricing/) [Azure Quantum — MS Learn](https://learn.microsoft.com/en-us/azure/quantum/)

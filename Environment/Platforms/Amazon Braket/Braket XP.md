#SoftDev #Python #Hardware 
`LocalSimulator` needs no AWS account, no credentials, no [[S3]]. Install the SDK & run circuits immediately:
```python
from braket.devices import LocalSimulator
from braket.circuits import Circuit

device = LocalSimulator()
result = device.run(Circuit().h(0).cnot(0, 1), shots=1000).result()
print(result.measurement_counts)  # {'00': 503, '11': 497}
```
Circuit syntax is clean method chaining. `LocalSimulator` handles up to $\sim 25$ [[Qubits]] & returns results instantly. This part works exactly as it looks.

Braket's unique strength: the same `Circuit` object runs on [[IonQ]] (trapped ion), [[Rigetti]] (superconducting), IQM (superconducting), & others with min adaptation. No other SDK gives unified access to this hardware variety. As of April $2026$, [[Rigetti]] Cepheus ($108$ [[Qubits]]) joined Ankaa-$3$ as the available [[Rigetti]] devices.

**QPU is always paid - no free hardware tier.** Unlike IBM ($10$ min/month free), Braket charges per task plus per shot on all QPUs. Free tier covers only $SV1$ simulator time for the first $12$ months, & only $1$ hour/month. LocalSimulator is always free.

**[[S3]] bucket is mandatory for cloud tasks & must be in the same region.** Cloud simulator & QPU results go to [[S3]] before being returned. If the bucket doesn't exist or is in the wrong region, the job fails silently with `ValidationException` or hangs. Create the bucket in AWS console before any cloud task. Region must match your Braket workspace region.

**Per-task fee is charged even if $0$ shots complete.** Job that fails after submission still incurs the task fee. Test exhaustively on `LocalSimulator` first. Set AWS billing alerts before any QPU experiment.

**QuEra Aquila is not gate-based.** It uses neutral atoms programmed via laser pulses & atom positions - `AnalogHamiltonianSimulation`, not `Circuit`. If you see Aquila in the device list expecting to run gate circuit on it, it will not work.

**`DM1` for noise, not `SV1`.** `SV1` is noiseless. To simulate realistic hardware errors, use `DM1` (Density [[Matrix]] simulator). `SV1` tells you if circuit logic is correct. `DM1` tells you if circuit will survive hardware noise.

**AWS account setup friction.** Getting from "no account" to "first cloud circuit" requires: AWS account, IAM permissions, [[S3]] bucket creation, Braket workspace, region configuration. More one-time setup than IBM or [[PennyLane]]. Worth it only if you specifically need multi-hardware comparison or AWS ecosystem integration.

**Billing escalates quickly on trapped-ion devices.** [[IonQ]] Forte & Aria are priced per gate-shot - deep circuits at high shot counts get expensive fast. Use Braket primarily to run final experiments you've already verified on simulators, not for exploratory debugging.

**Less learning material than IBM or [[PennyLane]].** Braket's documentation covers SDK usage well but has fewer algorithm tutorials & worked examples compared to [[IBM Quantum]] Learning or [[PennyLane]]'s demo gallery.
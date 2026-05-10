#SoftDev 
**[[Azure Quantum]] credits** are a free allowance provided to new users to explore real quantum hardware without upfront cost. New accounts receive a one-time credit usable with participating providers.

There are no standing charges for the [[Azure Quantum]] service itself - you only pay for what providers charge when jobs run on their hardware or simulators. [[QRE]] is completely free & requires no Azure account.

**Billing models vary by provider:**
- **[[IonQ]]** - per gate-shot ($1$-qubit & $2$-qubit gates $×$ shots), plus a minimum fee per program run.
- **[[Quantinuum]]** - per **Hardware Quantum Credit (HQC)**, calculated from operations $×$ shots.
- **[[Rigetti]]** - per second of QPU exec time; simulator (QVM) is free.
- **[[PASQAL]]** - per exec time on QPU or EMU-TN emulator.

Pricing is **pay-as-you-go** by default; some providers offer monthly subscription plans for heavier usage with discounted rates. Job cost estimates are shown before submission in the portal or SDK. Storage costs for the [[Azure Workspace|workspace's storage account]] are billed separately at standard Azure rates.

[[Azure Quantum]] uses **$3$ separate credit currencies**, each accepted by different providers:

| Currency                   | Abbreviation | Accepted by     |
| -------------------------- | ------------ | --------------- |
| [[Azure Quantum]] Credits  | AQC          | [[PASQAL]], [[Rigetti]] |
| [[Azure Quantum]] Tokens   | AQT          | [[IonQ]]            |
| $H$-System Quantum Credits | HQT          | [[Quantinuum]]      |
New accounts receive a **one-time credit equivalent** usable with participating providers. Credits do **not** expire. A separate **30-day free trial** period may be available depending on account type.

**[[Azure Quantum]] Resource Estimator** - completely free, no Azure subscription needed. Storage costs for the workspace storage account are billed separately at standard Azure Blob Storage rates.

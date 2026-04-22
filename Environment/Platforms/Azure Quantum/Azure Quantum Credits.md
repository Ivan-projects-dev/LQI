#Quantum #Cloud #Pricing
**[[Azure Quantum]] credits** are free allowance provided to new users to explore real quantum hardware without upfront cost. New accounts receive $1$-time **$500$ USD credit** usable with participating providers.

There are no standing charges for the [[Azure Quantum]] service itself - you only pay for what providers charge when jobs run on their hardware or simulators. [[QRE]] is completely free & requires no Azure account.

**Billing models vary by provider:**
- **IonQ** - per gate-shot ($1$-qubit & $2$-qubit gates $×$ shots), $+$ min fee per program run ($1–5$ USD).
- **Quantinuum** - per **Hardware Quantum Credit (HQC)**, calc from operations $×$ shots.
- **Rigetti** - per second of QPU exec time; simulator (QVM) is free.
- **PASQAL** - per exec time on QPU or EMU-TN emulator.

Pricing is **pay-as-you-go** by default; some providers offer monthly subscription plans for heavier usage with discounted rates. Job cost estimates are shown before submission in the portal or SDK. Storage costs for the [[Azure Quantum Workspace|workspace's storage account]] are billed separately at standard Azure rates.

## Credit & Token Types

[[Azure Quantum]] uses **three separate credit currencies**, each accepted by different providers:

| Currency | Abbreviation | Accepted by |
|---|---|---|
| [[Azure Quantum]] Credits | AQC | PASQAL, Rigetti |
| [[Azure Quantum]] Tokens | AQT | IonQ |
| H-System Quantum Credits | HQT | Quantinuum |

New accounts receive a **one-time $500 USD equivalent** usable with participating providers. Credits do **not** expire. A separate **30-day free trial** period may be available depending on account type.

## Updated Pricing Notes (2025)

**IonQ (AQT)** - pricing formula: min fee per execution + per-gate-shot charges. Min fee ≈$12.42 ([[Error Mitigation]] off) or ≈$97.50 ([[Error Mitigation]] on). Single-qubit gate: ≈$0.00022/shot; 2-qubit gate: ≈$0.00098/shot. Monthly subscription plans available at discounted rates for heavy users.

**Quantinuum (HQT)** - Standard & Premium subscription plans with monthly HQC allotments; Pay-As-You-Go available via Quantinuum sales (contact Sales@Quantinuum.com). HQC formula: sum(operation_cost × shots) per job.

**Rigetti (AQC)** - QPU charged per second of exec time only. QVM simulator is free. No per-shot or per-gate overhead.

**PASQAL (AQC)** - billed per exec time on both QPU (Fresnel) & EMU-TN emulator.

**[[Azure Quantum]] Resource Estimator** - completely free, no Azure subscription needed.

Storage costs for the workspace storage account are billed separately at standard Azure Blob Storage rates.

## Sources
- [Pricing plans for Azure Quantum providers](https://learn.microsoft.com/en-us/azure/quantum/pricing)
- [Azure Quantum Credits details](https://github.com/MicrosoftDocs/quantum-docs/blob/main/articles/azure-quantum-credits.md)
- [Azure Quantum pricing page](https://azure.microsoft.com/en-us/pricing/details/azure-quantum/)

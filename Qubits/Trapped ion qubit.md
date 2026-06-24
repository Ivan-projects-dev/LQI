#Qubit #Hardware
**Trapped-ion qubits** encode quantum information in the electronic energy levels of individual ions (typically $^{171}$Yb$^+$ or $^{40}$Ca$^+$) suspended in a **Paul trap** using oscillating electric fields. Among the leading qubit modalities for high-fidelity, low-noise computation.

### Hardware
- **Trapping**: Radio-frequency electric fields confine ions in a linear chain (1D crystal). Typical trap frequencies: axial ~1 MHz, radial ~2–5 MHz.
- **Qubit encoding**: Hyperfine ground states (Yb$^+$: $|0\rangle = |F=0\rangle$, $|1\rangle = |F=1\rangle$, $\Delta f \approx 12.6$ GHz) or optical transitions (Ca$^+$: $S_{1/2}$–$D_{5/2}$).
- **Gates**: Single-qubit via microwave/laser pulses. Two-qubit via **Mølmer–Sørensen (MS) gate**: shared phonon bus mediates entanglement — laser drives bichromatic field coupling all ion pairs through collective motional mode.
- **Measurement**: State-dependent fluorescence — $|0\rangle$ fluoresces, $|1\rangle$ dark (or vice versa). Photon counts discriminate states with more than 99.9% fidelity.

### Performance table
| Metric | Trapped ion | Superconducting |
|---|---|---|
| 1-qubit gate time | 1–10 μs | 10–50 ns |
| 2-qubit gate time | 100–600 μs | 100–500 ns |
| Coherence time | Seconds–minutes | 100 μs–1 ms |
| 1Q gate fidelity | more than 99.9% | more than 99.9% |
| 2Q gate fidelity | ~99.5% | ~99.5% |
| Connectivity | **All-to-all** | Nearest-neighbor |
| Qubit count (2025) | 32–56 (IonQ Forte, Quantinuum H2) | 100–1000+ |

### Key advantage: all-to-all connectivity
Every qubit can directly interact with every other qubit via the shared motional bus. No SWAP overhead for routing. Circuits that are deep on superconducting hardware due to routing may be shallow on trapped ion.

### Platforms
- **IonQ** (Yb$^+$): available via Amazon Braket (`ionq/ionQdevice`, `ionq/aria-1`, `ionq/forte-1`) and Azure Quantum
- **Quantinuum** (Ca$^+$, H-series): available via Azure Quantum; supports **Adaptive RI** profile (mid-circuit measurement + classical feedback). H2 processor: 56 qubits, all-to-all.

### When to choose trapped ion
- Circuit has long-range two-qubit gates that would require many SWAPs on superconducting
- Fidelity is the bottleneck (not gate speed)
- Mid-circuit measurement / feedforward required (Quantinuum H-series)
- Moderate qubit count, deep circuits

### Limitations
- **Slow gates**: ~1000× slower than superconducting — limits clock speed
- **Scale**: ion chains become unstable beyond ~50–100 ions in a single trap; QCCD (quantum charge-coupled device) architecture shuttles ions between zones to scale
- **Laser overhead**: complex optical setup, vibration sensitivity

Source: [IonQ documentation — Amazon Braket](https://docs.aws.amazon.com/braket/latest/developerguide/braket-devices.html) [Quantinuum H-Series — Azure Quantum](https://learn.microsoft.com/en-us/azure/quantum/provider-quantinuum)

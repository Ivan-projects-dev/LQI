#Quantum #[[FTQC]] #NISQ
**Error mitigation** is a collection of classical post-processing & circuit-modification techniques to reduce the effect of noise in **NISQ** (Noisy Intermediate-Scale Quantum) computations — without the qubit overhead of full [[FTQC|fault-tolerant]] error correction. It does not eliminate errors but extrapolates or cancels their effect on expectation values.

Key distinction: error *correction* uses redundancy to restore the [[Quantum state]] mid-circuit; error *mitigation* uses extra classical computation after many circuit runs to obtain a better estimate of the ideal result.

## Zero-Noise Extrapolation (ZNE)

Most widely used technique. Intentionally amplify noise by integer factors $\lambda = 1, 2, 3, \ldots$ (by gate folding or pulse stretching), measure expectation value $\langle O \rangle_\lambda$ at each noise level, then extrapolate back to $\lambda = 0$ (zero-noise [[Limit]]) via polynomial or Richardson extrapolation.

**Gate folding**: replace gate $G$ with $G \cdot G^\dagger \cdot G$ to double circuit noise without changing the ideal unitary.

**Limitation**: requires the noise to be smooth & well-behaved. Breaks down for highly structured errors or deep circuits.

IonQ exposes a native **error mitigation toggle** on [[Azure Quantum]] (affects pricing — on: ≈$97.50 min fee, off: ≈$12.42 min fee). See [[Azure Quantum Providers]].

## Probabilistic Error Cancellation (PEC)

Models the noise channel as a quasi-[[Probability distribution]] over ideal operations. Samples from corrective inverse operations to statistically cancel noise. Produces an unbiased estimator of the noise-free expectation value.

**Cost**: exponential in circuit depth × error rate — overhead grows as $e^{2\gamma L}$ where $\gamma$ is the noise strength and $L$ is circuit length. Practical only for shallow circuits.

## Dynamical Decoupling (DD)

Inserts sequences of carefully timed refocusing pulses (e.g., XY-4, CPMG sequences) into idle periods of [[Qubits]] to average out low-frequency noise (1/f noise, crosstalk). Does not require extra shots; works at the pulse level. Particularly effective for trapped-ion and superconducting platforms.

Recent work (2025): scalable DD combined with Hadamard phase cycling to filter non-Markovian noise dynamics.

## Clifford Data Regression (CDR)

Trains a classical model on near-Clifford circuits (which can be classically simulated exactly) to learn the noise bias, then applies the correction to the target non-Clifford circuit. Requires a classically simulable reference dataset.

## Symmetry Verification

If the target Hamiltonian has known symmetries (e.g., particle number conservation, parity), post-select shots that respect the symmetry. Discards erroneous shots that violate the known constraints. Simple & zero overhead, but reduces effective shot count.

## Mitiq (Open-Source Library)

Mitiq (Unitary Fund) provides a unified Python interface for ZNE, PEC, DD, CDR, and more. Works with Qiskit, Cirq, PyQuil, and [[Azure Quantum]] backends:

```python
from mitiq import zne

def run_on_azure(circuit):
    job = target.submit(circuit, shots=1000)
    job.wait_until_completed()
    return job.get_results()

mitigated_result = zne.execute_with_zne(circuit, executor=run_on_azure)
```

## Sources
- [Quantum error mitigation in the NISQ era (Medium, 2025)](https://medium.com/@raktims2210/quantum-error-mitigation-in-the-nisq-era-87af568290e5)
- [arXiv: Scalable QEM for dynamical decoupling (2511.12227)](https://arxiv.org/abs/2511.12227)
- [Mitiq documentation](https://mitiq.readthedocs.io/)
- [WERQSHOP 2025 report](https://unitary.foundation/assets/WERQSHOP_report.pdf)
- [IonQ provider pricing](https://learn.microsoft.com/en-us/azure/quantum/provider-ionq)

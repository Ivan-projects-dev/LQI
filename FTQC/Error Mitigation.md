#Math #Algorithm 
**Error mitigation** is collection of classical post-processing & circuit-modification techs to reduce the effect of noise in **NISQ (Noisy Intermediate-Scale Quantum)** computations - without the qubit overhead of full fault-tolerant error correction. It does not eliminate errors but extrapolates or cancels their effect on expectation values.
- **error correction** uses redundancy to restore the [[Quantum state]] mid-circuit; 
- **error mitigation** uses extra classical computation after many circuit runs to obtain better estimate of the ideal result.

**Zero-Noise Extrapolation (ZNE)** - most widely used tech. Intentionally amplify noise by int factors $\lambda = 1, 2, 3, \ldots$ (by gate folding or pulse stretching), measure expectation value $\langle O \rangle_\lambda$ at each noise level, then extrapolate back to $\lambda = 0$ ($0$-noise Limit) via polynomial or Richardson extrapolation.

**Gate folding**: replace gate $G$ with $G \cdot G^\dagger \cdot G$ to double circuit noise without changing the ideal unitary.

**Limitation**: requires the noise to be smooth & well-behaved. Breaks down for highly structured errors or deep circuits.

**Probabilistic Error Cancellation (PEC)** models the noise channel as a quasi-[[Probability distribution]] over ideal operations. Samples from corrective inverse operations to statistically cancel noise. Produces an unbiased estimator of the noise-free expectation value.

**Cost**: exponential in circuit depth $×$ error rate - overhead grows as $e^{2\gamma L}$ where $\gamma$ is the noise strength & $L$ is circuit length. Practical only for shallow circuits.

**Dynamical Decoupling (DD)** inserts sequences of carefully timed refocusing pulses (e.g., XY-4, CPMG sequences) into idle periods of [[Qubits]] to average out low-frequency noise (1/f noise, crosstalk). Does not require extra shots; works at the pulse level. Particularly effective for trapped-ion & superconducting platforms.

**Clifford Data Regression (CDR)** trains classical model on near-Clifford circuits (which can be classically simulated exactly) to learn the noise bias, then applies the correction to the target non-Clifford circuit. Requires a classically stimulable reference dataset.

### Symmetry Verification
If the target Hamiltonian has known symmetries (e.g., particle num conservation, parity), post-select shots that respect the symmetry. Discards erroneous shots that violate the known constraints. Simple & zero overhead, but reduces effective shot count.

**Mitiq (Unitary Fund)** provides unified Python interface for ZNE, PEC, DD, CDR, & more. Works with Qiskit, Cirq, PyQuil, & [[Azure Quantum]] backends:
```python
from mitiq import zne

def run_on_azure(circuit):
    job = target.submit(circuit, shots=1000)
    job.wait_until_completed()
    return job.get_results()

mitigated_result = zne.execute_with_zne(circuit, executor=run_on_azure)
```

Source: [Mitiq documentation](https://mitiq.readthedocs.io/en/stable/) [Mitiq — GitHub](https://github.com/unitaryfund/mitiq)

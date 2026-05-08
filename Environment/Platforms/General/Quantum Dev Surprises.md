#Physics #Hardware #Algorithm Quantum #SoftDev Surprises
###  1. Simulation & Hardware Give Very Different Results
Circuit that gives perfect `00`/`11` in simulation gives `490/507/3/4` on real hardware. $7$ wrong answers are not software bug - they are physical errors in the quantum chip. Every gate has small error probability; every qubit decoheres over time. This gap grows **rapidly** with circuit depth. $3$-gate circuit looks nearly perfect on hardware. $50$-gate circuit may be dominated by noise.

This is not temporary problem but fundamental challenge of the **NISQ (Noisy Intermediate-Scale Quantum)** era.
###  2. You Run the Same Circuit Many Times, Not Once
$1$ shot gives $1$ classical bit string (e.g., `01`). That tells almost nothing. $1024$ shots gives you distribution. Distribution is what carries the quantum info.

Common mistake: expecting single "correct answer" from quantum program. Instead, you get distribution & infer the answer from the structure of that distribution.
###  3. $>$ Qubits is not better
$5$-gate, $2$-qubit circuit will outperform $50$-gate, $5$-qubit circuit on real hardware. Every gate adds error. Every [[SWAP]] gate inserted during routing adds $3$ CNOTs. Community calls this the **circuit depth vs. fidelity tradeoff**.

On simulators depth is free. On hardware it costs you in noise. Optimization for real hardware means minimizing depth, not maximizing qubit count.
###   4. Quantum Advantage is not general-purpose
Quantum computers are faster at specific problem types, not everywhere:
- Factoring large nums $→$ [[Shor]] (requires fault-tolerant hardware that doesn't exist yet)
- Searching unsorted DBs $→$ [[Grover]] algorithm (quadratic speedup, limited practical impact)
- Simulating quantum chemistry $→$ [[VQE]] (near-term relevant)
- Certain optimization problems $→$ [[QAOA]] (still under research)

For sorting, web requests, video games, spreadsheets, DBs - classical computers are & will remain faster. Current quantum hardware is best understood as research instrument, not speed upgrade.
###  5. Queue Times are part of the experiment
On [[IBM Quantum]], submitting job & getting results back can take $30$ minutes to $3$ hours during peak times. This is not bug - there are $>$ users than QPU time available. Design your workflow around asynchronous execution: submit batch, come back later, analyze.

`least_busy()` helps but doesn't eliminate waiting.
###  6. Resource estimator will humble you
Run [[Azure Quantum]]'s [[QRE]] on any textbook algorithm. [[Shor]] algorithm at [[RSA]]-$2048$ scale needs $~4$ million physical [[Qubits]] & years of runtime on hardware that doesn't exist yet. Most "quantum algorithms" in textbooks describe math procedures, not programs you can run today.
###  7. D-Wave Is Not the Same Type of Computer
[[D-Wave]] is **quantum annealer**, not gate-based computer. It does not run circuits. It cannot execute [[Grover]], [[Shor]], [[VQE]], or [[QPE]]. It is optimizer for combinatorial problems. Comparing [[D-Wave]] to IBM is like comparing graphics card to CPU - different tools for different jobs.
###  Sources
- [IBM Quantum: understanding noise](https://docs.quantum.ibm.com/guides/noise-learning)
- [Qiskit: error mitigation overview](https://docs.quantum.ibm.com/guides/error-mitigation-and-suppression-techniques)
- [Azure Resource Estimator intro](https://learn.microsoft.com/en-us/azure/quantum/intro-to-resource-estimation)
- [NISQ era overview - Preskill 2018](https://arxiv.org/abs/1801.00
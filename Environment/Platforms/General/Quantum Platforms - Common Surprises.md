#Quantum #Beginner #NISQ
Things that consistently catch beginners off-guard across all quantum platforms.

## Simulation and Hardware Give Very Different Results

A circuit that gives perfect `00`/`11` in simulation gives `490/507/3/4` on real hardware. The $7$ wrong answers are not a software bug - they are physical errors in the quantum chip. Every gate has a small error probability; every qubit decoheres over time. This gap grows **rapidly** with circuit depth. A $3$-gate circuit looks nearly perfect on hardware. A $50$-gate circuit may be dominated by noise.

This is not a temporary problem waiting to be engineered away - it is the fundamental challenge of the NISQ ([[NISQ|Noisy Intermediate-Scale Quantum]]) era.

## You Run the Same Circuit Many Times, Not Once

One shot gives you $1$ classical bit string (e.g., `01`). That tells you almost nothing. $1024$ shots gives you a distribution. The distribution is what carries the quantum information.

Think of it like rolling a die. $1$ roll doesn't tell you the die is fair. $1000$ rolls shows you the distribution. The same [[Logic]] applies to quantum circuits.

Common mistake: expecting a single "correct answer" from a quantum program. Instead, you get a distribution and infer the answer from the structure of that distribution.

## More Qubits Is Not Better - Fewer Gates Is

A $5$-gate, $2$-qubit circuit will outperform a $50$-gate, $5$-qubit circuit on real hardware. Every gate adds error. Every SWAP gate inserted during routing adds $3$ CNOTs. The community calls this the **circuit depth vs. fidelity tradeoff**.

On simulators depth is free. On hardware it costs you in noise. Optimization for real hardware means minimizing depth, not maximizing qubit count.

## Quantum Advantage Is Not General-Purpose

Quantum computers are not faster at everything. They are potentially faster at specific problem types:
- Factoring large numbers → [[Shor's algorithm]] (requires fault-tolerant hardware that doesn't exist yet)
- Searching unsorted databases → [[Grover's algorithm]] (quadratic speedup, limited practical impact)
- Simulating quantum chemistry → [[VQE]] (near-term relevant)
- Certain optimization problems → [[QAOA]] (still under research)

For sorting, web requests, video games, spreadsheets, databases - classical computers are and will remain faster. Current quantum hardware is best understood as a research instrument, not a speed upgrade.

## Queue Times Are Part of the Experiment

On [[IBM Quantum]], submitting a job and getting results back can take $30$ minutes to $3$ hours during peak times. This is not a bug - there are more users than QPU time available. Design your workflow around asynchronous execution: submit a batch, come back later, analyze.

`least_busy()` helps but doesn't eliminate waiting.

## The Resource Estimator Will Humble You

Run [[Azure Quantum]]'s [[Quantum resource estimator]] on any textbook algorithm. Shor's algorithm at RSA-2048 scale needs roughly $4$ million physical [[Qubits]] and years of runtime on hardware that doesn't exist yet. Most "quantum algorithms" in textbooks describe mathematical procedures, not programs you can run today.

This is not pessimism - it is the current state of the field. Understanding the gap between theoretical quantum algorithms and real hardware is crucial context.

## D-Wave Is Not the Same Type of Computer

[[D-Wave Leap]] is a **quantum annealer**, not a gate-based computer. It does not run circuits. It cannot execute [[Grover]], Shor, [[VQE]], or [[QPE]]. It is an optimizer for combinatorial problems. Comparing D-Wave to IBM is like comparing a graphics card to a CPU - different tools for different jobs.

## Sources
- [IBM Quantum: understanding noise](https://docs.quantum.ibm.com/guides/noise-learning)
- [Qiskit: error mitigation overview](https://docs.quantum.ibm.com/guides/error-mitigation-and-suppression-techniques)
- [Azure Resource Estimator intro](https://learn.microsoft.com/en-us/azure/quantum/intro-to-resource-estimation)
- [NISQ era overview - Preskill 2018](https://arxiv.org/abs/1801.00862)

#Quantum #Algorithm 
**Variational Quantum Algorithms (VQAs)** are a class of hybrid quantum-classical algorithms designed to harness the power of quantum computing while addressing the challenges posed by current quantum hardware.

These algorithms are particularly promising for near-term quantum devices, known as **Noisy Intermediate-Scale Quantum (NISQ) devices**, which are limited by noise, short coherence times, & relatively small nums of [[Qubits]]. 

At their core, VQAs combine quantum & classical computing resources in an iterative process to solve complex problems.  In this module, we will explore the principles behind VQAs, the role of quantum & classical components, & how these algorithms are implemented on current quantum hardware.

**Parametrized quantum circuits** consist of quantum gates that have tunable params, allowing the [[Quantum state]] to be manipulated during optimization.
![[Pasted image 20260101164554.png]]
In VQAs, a quantum circuit is init with some params, & the output of the circuit is measured. These measurement results are used to compute a cost func, which tells us how good the current params are. A classical optimizer then adjusts the params to improve the result.
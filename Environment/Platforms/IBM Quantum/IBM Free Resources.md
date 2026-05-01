#SoftDev [[IBM Quantum]] has the most mature free access program for actual QPU time among all platforms. 

| Resource                  | Account needed | What you get                                        |
| ------------------------- | -------------- | --------------------------------------------------- |
| IBM Quantum free QPU      | IBM account    | $~10$ min/month on real hardware (shared queue)     |
| AerSimulator (local)      | None           | Unlimited statevector / noisy simulation            |
| IBM Quantum Learning      | None           | Structured courses with Q&A & exercises             |
| Qiskit Textbook           | None           | Open-source, full algorithm implementations         |
| IBM Quantum Lab (Jupyter) | IBM account    | Jupyter in the cloud, no local install              |
| FakeBackends              | None           | Offline noise simulation from real calibration data |
### IBM Quantum Learning
**Access:** [learning.quantum.ibm.com](https://learning.quantum.ibm.com) - no account required to read, IBM account to track progress.
**Available courses (all free):**

| Course                             | Level | Content |
|---|---|---|
| Basics of Quantum info             | Beginner | States, gates, circuits, measurement |
| Fundamentals of Quantum Algorithms | Intermediate | [[Grover]], QFT, [[QPE]], [[Shor]], [[VQE]] |
| Quantum Error Correction           | Advanced | Stabilizer codes, [[Surface Code]], fault tolerance |
| Variational Algorithm Design       | Intermediate | [[VQE]], [[QAOA]], parameter optimization |
| Utility-Scale Quantum Computing    | Advanced | [[Error Mitigation]], large circuits |
Each course has:
- Video explanations
- Inline Jupyter notebooks (run in browser via [[IBM Quantum]] Lab)
- Auto-graded exercises
- Source code for all circuits

**Most useful starting path:**
1. *Basics of Quantum info* - complete it. It covers the math foundations (Dirac notation, [[Tensor]] products, measurement) that every other resource assumes.
2. *Fundamentals of Quantum Algorithms* - [[Grover]] & [[QPE]]. Once you've done these two, you can read most quantum algorithm papers.
## Sources
- [IBM Quantum: Getting started](https://docs.quantum.ibm.com/start)
- [IBM Quantum Learning](https://learning.quantum.ibm.com)
- [Qiskit documentation](https://docs.quantum.ibm.com)
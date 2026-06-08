#Algorithm
**Variational Quantum Algorithms (VQAs)** are hybrid quantum-classical algorithms that run a parameterized quantum circuit $U(\theta)$ on a quantum processor, measure an observable $\langle \psi(\theta) | H | \psi(\theta) \rangle$, and update $\theta$ classically to minimize the cost. The quantum processor evaluates the cost function; a classical optimizer (COBYLA, Adam, SPSA) updates parameters.

**Motivation**: on NISQ devices, deep circuits accumulate too much gate error. VQAs use shallow circuits (low depth) and accept that the ansatz $U(\theta)|0\rangle$ may not reach the exact ground state — close is good enough for some applications.

**Loop**:
1. Prepare $|\psi(\theta)\rangle = U(\theta)|0^n\rangle$ on QPU.
2. Measure cost $C(\theta) = \langle \psi(\theta) | H | \psi(\theta) \rangle$ via repeated shots.
3. Classical optimizer computes $\nabla_\theta C$ (via parameter-shift rule: $\partial C / \partial \theta_k = \frac{1}{2}[C(\theta_k + \pi/2) - C(\theta_k - \pi/2)]$).
4. Update $\theta \leftarrow \theta - \eta \nabla_\theta C$. Repeat.

**Instances**: [[VQE]] minimizes molecular energy (chemistry); [[QAOA]] approximates combinatorial optimization; VQLS solves linear systems; QCNN/QFNN are VQAs for ML.

**Known problems**:
- **Barren plateaus**: for random or deep ansätze, $\text{Var}[\partial C / \partial \theta_k] \propto 2^{-n}$ — gradients vanish exponentially, making optimization intractable. Affects all VQAs at scale.
- **Local minima**: non-convex landscape; classical optimizer can get stuck.
- **Measurement overhead**: estimating $C(\theta)$ to precision $\epsilon$ requires $O(1/\epsilon^2)$ shots. For many-term Hamiltonians ($> 10^5$ Pauli terms in chemistry), this is the dominant cost.
- **No proven quantum advantage**: for most VQA instances, classical algorithms (DMRG, tensor networks) match or outperform VQAs on current hardware sizes.
![[Pasted image 20260101164554.png]]

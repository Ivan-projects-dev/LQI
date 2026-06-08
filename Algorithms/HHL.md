#Algorithm #Math #ML
In $2009$, Harrow, Hassidim & Lloyd (HHL) proposed quantum algorithm for solving sparse, well-conditioned linear systems $A\vec{x} = \vec{b}$, with gate complexity $O(\kappa^2 \log N / \epsilon)$ vs classical $O(N\kappa)$ - an exponential improvement in $N$ under the right conditions.

HHL is used as a subroutine in quantum algorithms for differential equations, quantum ML ([[QPCA]], [[QSVM]]), & optimization. See [[QPE HHL]] for the full circuit & worked example.

**Critical assumptions - speedup only holds when:**
- $A$ is **sparse** (few non-zero entries per row) & efficiently stimulable as $e^{iAt}$
- **Condition num $\kappa$ is small** - complexity scales as $O(\kappa^2)$; ill-conditioned systems lose the advantage entirely
- $|b\rangle$ can be prepared efficiently - loading classical $\vec{b}$ into [[Quantum state]] costs $O(N)$ without **QRAM**
- The answer can be extracted via $O(1)$ expectation values - reading out the full solution $\vec{x}$ destroys the speedup (costs $O(N)$ measurements)

In practice, these conditions are rarely all satisfied simultaneously for classical data. Algorithm's impact is primarily as subroutine within larger quantum workflows operating on natively quantum data.

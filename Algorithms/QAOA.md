#Quantum #Algorithm 
**Quantum approx optimization algorithm (QAOA)** is a hybrid algorithm for solving combinatorial optimization problems. 

In combinatorial optimization problems, we look for the best solution among a finite set of solutions. The solution space is often exponentially large. The solution space being finite & discrete makes it harder to find the best solution. For instance, although there are efficient methods for continuous optimization problems which have infinite solution space, many of the combinatorial optimization problems belong to the class $NP$-hard.

In QAOA, the goal is to find the ground state of HC, which is called is the problem or [[Cost Hamiltonian]] Graph that QAOA is an approx algorithm & it does not guarantee the optimal solution.

Consider a func f defined on n bits. Suppose that the Hamiltonian HC satisfies

$HC|x⟩=f(x)|x⟩$, for $x∈{0,1}n$. The ground state of $HC$ corresponds to $x$ that minimizes Max-Cut & the ground state energy is the min value of $f(x)$.

Therefore, minimizing the expectation value of $HC$ is equivalent to minimizing $f(x)$.

We will look at how to express a combinatorial optimization problem as a func of bit strings $f(x)$ and how to obtain the corresponding Hamiltonian $HC$ in the next notebook. Let us note that such Hamiltonians are always diagonal.

In QAOA, the following [[Quantum state]] serves as our ansatz:
$$|ψ(γ,β)⟩=U(HB,βp)U(HC,γp)…U(HB,β1)U(HC,γ1)|s⟩$$
where $$U(HB,βp)=exp(−iβpHB)$
$$$$(HC,γp)=exp(−iγpHC)$$& $p$ - num of steps or layers that determines how many times the unitaries $$U(HB,βi)U(HC,γi) $$ are applied. $HB$ is the [[Mixer Hamiltonian]] defined as $−∑n_iX_i$ & $|s⟩=|+⟩n$ is init state which is the ground state of HB. $β$ and $γ$ form a set of $2_p$ params.

Overall, we can express the QAOA ansatz as: $$p∏i=1exp(−iβpHB)exp(−iγpHC)|+⟩n$$
In QAOA, we start with a set of init params, & then iteratively measure the [[Quantum state]], & update the params with the help of a classical optimizer so that our cost func, which is equal to the expectation value of HC in this case, is minimized. Note that the params $β_p$ and $γ_p$ determine the duration we evolve the state by the Hamiltonians HB and HC.

In QAOA as introduced by Farhi et al., $HB=−∑n_iX_i$ and the init state is picked as its ground state $|s⟩=|+⟩n$.

In general, $1$ can pick different HB as long as it does not commute with HC and pick the init state accordingly as the ground state of HB. This variant is called Quantum Alternating Operator Ansatz.

We denote the params at iteration $i$ by $(γi,βi)$.
- We construct our ansatz $$∏pi=1exp(−iβpHB)exp(−iγpHC)|+⟩n$$.
- We init its params to obtain $|ψ(γ_0,β_0)⟩$.
- We use a classical optimizer to adjust the params $γ$ and $β$ to minimize our cost func. The cost func is computed by the outcomes of the measurement results & its the minimization corresponds to minimization of $$⟨ψ(γ_i,β_i)|HC|ψ(γ_i,β_$i)⟩$$
QAOA is a hybrid algorithm, where the optimization of the params is performed on a classical computer, & the quantum computer is used for computing the cost func.
![[Pasted image 20260102010423.png]]

| Advantages                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Disadvantages                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Near-Term Applicability**: QAOA is designed for near-term quantum devices as it doesn't require full fault tolerance & can be run with relatively shallow circuits.<br>**Problem-Specific Ansatz**: QAOA can be tailored to specific optimization problems by adjusting the struct of the quantum circuits, making it good fit for wide variety of combinatorial optimization problems.<br>**Hybrid Approach**: Like [[VQE]], QAOA is hybrid quantum-classical algorithm, with the quantum device handling the quantum part & classical computer performing the optimization. | **Measurement Overhead**: Similar to [[VQE]], QAOA requires many measurements to estimate the expectation values of operators, resulting in significant measurement overhead, which can increase the time & cost of the computation.<br>**Optimization Challenges**: optimization process in QAOA can be challenging, especially due to issues like barren plateaus or the difficulty of navigating the complex cost landscape with classical optimization methods.<br>**Ansatz Depth Trade-offs**: performance of QAOA improves with deeper circuits ($>$ layers of quantum gates), but deeper circuits are difficult to implement on NISQ devices due to gate errors & noise. Additionally, deeper circuits might not always provide significant improvement in optimization performance. |

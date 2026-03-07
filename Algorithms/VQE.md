#Quantum #Algorithm 
**Variational Quantum Eigensolver (VQE)** is a quantum algorithm designed to approx (find an upper bound) the _ground-state energy_ of a quantum system. 

**Quantum system** can exist in multiple energy states, each associated with a specific energy level. The ground state is the lowest energy state of the system. It is the most stable config because, unless external energy is applied, a system will always tend to settle in its ground state.

Ground state energy corresponds to the lowest eigenvalue of the _Hamiltonian_ that represents the system.

Pauli Z [[Matrix]] represents a single-qubit Hamiltonian that can describe the energy associated with the spin of a qubit along the z-axis. 
$σZ=(100−1)$.
It is Hermitian & its lowest eigenvalue is $-1$ with the corresponding eigenstate $(01)$.

Pauli matrices form a complete set of operators for [[Qubits]], meaning that any operator acting on a qubit system can be expressed as a linear combo of these Pauli operators. Hence, a general Hamiltonian for a quantum system can be written as a sum of terms called _Pauli strings_, each of which involves Pauli matrices acting on one or $>$ [[Qubits]].

As an example, we can define the following Hamiltonian on two [[Qubits]]: $X_0⊗X_1+Y_0⊗Y_1+Z_0⊗Z_1$, typically written as $XX+YY+ZZ$. In this example, $XX, YY$ and $ZZ$ are **Pauli strings**. This Hamiltonian is known as the Heisenberg Hamiltonian. Its [[Matrix]] representation can be obtained through the matrices of Pauli $X, Y$, and $Z$ operators.

Here are some other random Hamiltonians: $2XIX−YZI$, $3XYY+2ZZI−4YXY$, $−XI+YZ+5XX$. The coefficients represent the strength of the interaction between different Pauli operators acting on the [[Qubits]], with both positive & negative values indicating different types of interactions (attractive or repulsive) between [[Qubits]].

For a Hamiltonian H with eigenstates $|ψλ⟩$ and corresponding eigenvalues $Eλ$, the variational principle states that for any normalized trial state $|ψ⟩$, the expectation value of $H$ satisfies:

$E0≤⟨ψ|H|ψ⟩$, where $E0$ is the ground-state energy (the lowest eigenvalue of $H$).

Now let us understand why this is the case. We can express the expectation value as follows.

Since any state can be expressed as a linear combo of the eigenstates $⟨ψ|H|ψ⟩=⟨∑λc∗λψλ|H|∑μcμψμ⟩=∑λ,μc∗λcμ⟨ψλ|H|ψμ⟩$.

Now since $H|ψλ⟩=Eλ|ψλ⟩$, we get: $⟨ψ|H|ψ⟩=∑λ,μc∗λcμEμ⟨ψλ|ψμ⟩$.

Since the eigenstates are orthonormal, all terms vanish except for the case $μ=λ$.
$⟨ψ|H|ψ⟩=∑λ|cλ|2Eλ$

Since $E0$ is the lowest eigenvalue of $H$, it follows that: $E0∑λ|cλ|2≤∑λ|cλ|2Eλ=⟨ψ|H|ψ⟩$.

Since $∑λ|cλ|2=1$, this simplifies to: $E0≤⟨ψ|H|ψ⟩$.

This proves that the expectation value of $H$ for any state $|ψ⟩$ is always greater than or equal to the ground-state energy $E0$. In other words, any random guess of a [[Quantum state]] will provide an upper bound on the ground state energy. This principle forms the basis for the VQE, which aims to approx $E_0$ by optimizing over different trial states.

VQE works by iteratively preparing a trial [[Quantum state]] with adjustable params, measuring its energy on a quantum computer, & using classical optimization to refine the params until the lowest possible energy (ground state) is approx.

"Trial [[Quantum state]] with adjustable params" is the ansatz we discussed in the previous notebook. The quality of the ansatz directly affects the efficiency of the algorithm-too simple, & it may not capture the correct solution; too complex, & it may be difficult to optimize.

Common approaches include:
- **Hardware-Efficient Ansatz**: Uses native quantum gates but may suffer from barren plateaus.
- **Unitary Coupled Cluster (UCC) Ansatz**: Popular in quantum chemistry but requires deep circuits.
- **Problem-Specific Ansatz**: Tailored to a given Hamiltonian, often improving efficiency.
- **Adaptive Ansatz** (e.g., ADAPT-VQE): Dynamically constructs an optimal ansatz, reducing circuit depth.

In general, the Hamiltonian whose ground state energy we would like to estimate will consist of the composition of Pauli strings i.e., $H=H1+H2+⋯+Hn$. So to compute $⟨ψ|H|ψ⟩$, we can compute individual terms $⟨ψ|Hi|ψ⟩$ & take the sum.

Consider $H=2Y+Z−X$. The Hamiltonian consists of $3$ terms, $H1=2Y, H2=Z$, & $H3=−X$. To compute the expectation value of $ψ$ with respect to $H$, we need to prepare $3$ different circuits.

Suppose that our Hamiltonian is given by $Z_0Z_1$. Eigenstates of $Z_0Z_1$ are $|00⟩, |01⟩, |10⟩, |11⟩$, with the corresponding eigenvalues $1,−1,−1, 1$ respectively. How can we implement this behaviour?

Note that eigenvalues are $1$ if $2$ [[Qubits]] are in the same state, & $-1$ otherwise.

Let us implement a CNOT gate on the two [[Qubits]] as CNOT $|q0⟩|q1⟩=|q0⟩|q0⊕q1⟩$,

- $|q0⊕q1⟩=|0⟩$, if $2$ [[Qubits]] have the same state
- $|1⟩$, otherwise.

After applying a CNOT, measurement with Z on qubit q1 yields us the result we desire.

This idea can be extended to multiple [[Qubits]] by applying CNOT in a chain struct. Such CNOT chain keeps the parity of $1s$ in the state & results in state $|1⟩$ on the final target qubit if the num of $1s$ in the state is odd, & $|0⟩$ otherwise. Hence, a measurement on the final qubit with observable $Z$ coincides with the eigenvalues of the multiqubit observable.
!Pasted image 20260102005032.png
!Pasted image 20260102005009.png
- We select a parametrized quantum circuit $|ψ(θ)⟩$ to serve as our ansatz.
- We init its params $|ψ(θ0)⟩$.
- We use a classical optimizer to adjust the param $θ$, by performing measurements to compute $⟨ψ(θ0)|H|ψ(θ0)⟩$, which is the cost func we would like to minimize.
VQE is a hybrid algorithm, where the optimization of the params is performed on a classical computer, & the quantum computer is used for calc the expectation value.
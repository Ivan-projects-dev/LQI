#Math #Quantum 
We can express $U(T, 0)$ as $U(tn, tn−1)U(tn−1, tn−2)⋯U(t1,t0)=n∏k=1U(tk,tk−1))$, where $t_k=kδt (k=0,1,…,n)$, $t_n=T$, $t_0=0$, and $δ_t=T/n$. Here, we take small time steps $δ_t$ to discretize the continuous evolution. Note that this step is exact for a time independent Hamiltonian.

In our case where the Hamiltonian is time-dependent, during the small time step, we assume that the Hamiltonian is approx cons, & approx $U(t_k, t_(k−1))$ as

$U(t_k, t_(k−1))≈exp(−i_H(t_k)⋅δ_t)$, where $H(t_k)$ is the Hamiltonian at time $t_k$. Hence,

$U(T, 0)≈n∏k=1exp(−iH(kδ_t)⋅δ_t)$.

In the case of AQC, recall that the Hamiltonian at time $t$ is given by $H(t)=(1−tT)Hi+tTH_f$.
Replacing this in the above equation we get: $U(T,0)≈n∏k=1exp(−i((1−kδtT)Hi+kδtTHf)δt)$.

Now we use the **Trotter formula** which states that $ex(A+B)=exAexB+O(x^2)$ for non-commuting Hamiltonians $A$ and $B$.

$U(T,0)≈n∏k=1exp(−i(1−kδtT)δtHi)exp(−ikδtTδtHf)$.

This equation suggests that by applying the Hamiltonians Hi and HF iteratively, we can approx adiabatic evolution as $n$ gets larger & larger.

This is the inspiration behind QAOA, although it does not exactly mimic the equation above for $U(T,0)$.

Implementation of QAOA consist of $3$ parts:
- init state
- [[Mixer Hamiltonian]]
- [[Cost Hamiltonian]]

Since the init state is $|+⟩n$, we can implement it by applying $H$ to all [[Qubits]].
#Math #Quantum #Physics 
We have seen that in quantum computing, the evolution of closed system is described by unitary transformation.

That is, the state of the system $|ψ(t)⟩$ at time $t$ can be expressed as $|ψ(t)⟩=U(t,t0)|ψ(t0)⟩$,
where $U(t, t_0)$ is [[Unitary operator]] that depends on times $t$ and $t_0$.

An equivalent way of stating this fact is the **Schrödinger's equation** that provides continuous-time descr of the system while through Unitary operators, we have discrete-time descr: $H|ψ(t)⟩=iℏddt|ψ(t)⟩$, where $ℏ$ is physical const known as Planck’s const and $ddt$ is the derivate. In practice, it is common to absorb the factor $ℏ$ into $H$, the Hamiltonian describing the system.

The Schrödinger's equation is differential equation, whose solution is given by $|ψ(t)⟩=exp(−iH⋅(t−t0))|ψ(t0)⟩$.

One can prove that $e−iH(t−t0)$ is always unitary & any unitary can always be expressed as $e−iK$ for some Hermitian operator $K$. This proves the equivalence between the $2$ alternative ways of viewing time evolution in quantum systems.

In other words, if the state of system at time $t_0$ is $|ψ(t_0)⟩$, its state at time $t$ is given by $e−iH(t−t_0)|ψ(t_0)⟩$ and the corresponding unitary transformation is given by $U(t, t_0)=exp(−iH⋅(t−t_0))$.

Note that if $|ψ⟩$ is eigenstate of $H$, it only acquires phase when it is evolved by Hamiltonian $H$, as it is not possible to transition between eigenstates.
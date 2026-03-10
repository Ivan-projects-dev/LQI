#Math #Physics #Quantum 
**2-local cost Hamiltonian** is defined as  
$$H_C = \sum_i h_i Z_i + \sum_{i<j} J_{ij} Z_i Z_j$$
where:
- $Z_i$ is the Pauli-$Z$ operator acting on qubit $i$
- $h_i \in \mathbb{R}$ are local fields
- $J_{ij} \in \mathbb{R}$ are coupling constants
**Pauli-(Z) Eigenstates** $Z\ket{0} = +\ket{0}, \quad Z\ket{1} = -\ket{1}$
Define **spin variables**: $s_i \in [+1, -1]$ such that:  $Z_i \ket{s_i} = s_i \ket{s_i}$
For computational basis state $\ket{s} = \ket{s_1 s_2 \dots s_n}$ we have:  
$$H_C \ket{s}  
= \left( \sum_i h_i s_i + \sum_{i<j} J_{ij} s_i s_j \right) \ket{s}$$
Thus, $\ket{s}$ is **eigenstate** of $H_C$ with eigenvalue:  
$E(s) = \sum_i h_i s_i + \sum_{i<j} J_{ij} s_i s_j$
**Ising model energy func** is defined as:  
$$E_{\text{Ising}}(s)  
= \sum_i h_i s_i + \sum_{i<j} J_{ij} s_i s_j$$
Equivalence Statement $E_{\text{Ising}}(s) = \bra{s} H_C \ket{s}$
Therefore:
- Minimizing the **Ising energy**
- Is equivalent to finding the **ground state** of (H_C)

> Any optimization problem that can be mapped to **[[Ising model]]** can be directly implemented as **cost Hamiltonian** for quantum algorithms (e.g. [[QAOA]], [[Quantum annealing]]).

#Physics #Quantum #Math 
Electrons have quantum mechanical property called _spin_ which is the angular momentum of the particle. When measured, it is either $h/2$ (spin up) or $−h/2$ (spin down) where $h$ is Planck's const.

Electron’s spin is closely related to its magnetic moment so that an electron behaves like tiny bar magnet with North & South pole. **Ferromagnetism** arises when collection of atomic spins align such that their associated magnetic moments all point in the same direction, & the spins behave like big magnet with net macroscopic magnetic moment.
![[Pasted image 20260101205937.png]]

**Ising Model** is math model to study ferromagnetism in statistical physics. Ising model was first proposed by Wilhelm Lenz who gave it as problem to his graduate student Ernst Ising, after whom this model is named. 

Each spin takes either the value $s=1$ (up) or $s=−1$ down.

When spins are arranged on $1-D$ line, so that each spin interacts only with its right & left neighbors, the model is called the **1-Dimensional Ising Model**.
![[Pasted image 20260101205950.png]]
When the spins are arranged on $2-D$ lattice, each spin interacts with its right, left, up, & down neighbors, the model is also known as the **$2-D$ Ising model**.
![[Pasted image 20260101205920.png]]
Configuration of spins yielding the lowest energy is known as the **ground state**. It is NP-Hard to find the ground state of 2-D Ising model. Thus, finding the ground state is as hard as problems like the Max-Cut problem & the Travelling Salesperson problem. Spins can be arranged in any other config.

We would like to express the energy of every possible config of the spins in the system. We will assume that all possible couplings are possible between any $2$ spins. (For example, $Ji, j$ may exist for any $(i,j)$ pair. In realistic scenarios, it may not be possible to have coupling between any $2$ pair of [[Qubits]])

- Spins interact with the external magnetic field $h$, if present.
- Each spin state (var) interacts with the other spins. The _coupling strength_, of this spin-spin interaction, is characterized by the const $J$.
- Each spin var $s_i$ takes the values ${−1,1}$.

Based on those assumptions, the energy of the Ising Model is given by $$E_{\text{Ising}}(s) = \sum_i h_i s_i + \sum_{i<j} J_{ij} s_i s_j$$
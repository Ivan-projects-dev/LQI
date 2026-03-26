#Quantum #Math #ML
The elements of the [[Pauli group]] have another important property that relates to quantum states, which we will study now. In the next section, we will use the notions introduced here to construct stabilizer groups.

We will need to recall the eigenvectors & eigenvalues of $X,Y,Z$ are $X|+⟩=|+⟩,Y|+i⟩=|+i⟩Z|0⟩=|0⟩,X|−⟩=−|−⟩,Y|−i⟩=−|−i⟩,Z|1⟩=−|1⟩$.

In the equations above, the ones on the left are $+1$ eigenstates. This means acting on the state by the corresponding operator leaves the state unchanged (even accounting for a phase). We are going to give a special name to such states.

> Given $P∈P_n$, a state $|ψ⟩$ such that $P|ψ⟩=|ψ⟩$, is called a **stabilizer state** of $P$.

Consider, the Pauli operator $X⊗Z$. From the equations above, it is quite easy to see that $$(X⊗Z)|+⟩⊗|0⟩=|+⟩⊗|0⟩$$[[Tensor]] product of the $+1$ eigenvectors of the individual operators gives $a +1$ eigenvector of the [[Tensor]]-product operator. $$(X⊗Z)|−⟩⊗|1⟩=(−1)2|−⟩⊗|1⟩=|−⟩⊗|1⟩$$. In this way, we can see that the operator $X⊗Z$ has two +1 eigenvectors (it also has two $− 1$ eigenvectors; why?).

Of course, any state that is linear combo of $|+⟩⊗|0⟩$ & $|−⟩⊗|1⟩$ is also $a +1$ eigenstate of $X⊗Z$, $$(X⊗Z)(α|+⟩⊗|0⟩+β|−⟩⊗|1⟩)=α(X⊗Z)|+⟩⊗|0⟩+β(X⊗Z)|−⟩⊗|1⟩=α|+⟩⊗|0⟩+β|−⟩⊗|1⟩$$
Similarly, for any Pauli operator $P$ in $P_n$, we can construct states that are the $+1$ eigenvectors of the operator.$$(X⊗I⊗X)|+⟩⊗|ϕ⟩⊗|+⟩=|+⟩⊗|ϕ⟩⊗|+⟩$$,$$(X⊗I⊗X)|−⟩⊗|ϕ⟩⊗|−⟩=|−⟩⊗|ϕ⟩⊗|−⟩$$$I$ is special because every state $|ϕ⟩$ is $a + 1$ eigenvector of $I$. Since $|ϕ⟩$ is arbitrary, we infer that $X⊗I⊗X$ stabilizes all states that are linear combo of $${|+⟩⊗|0⟩⊗|+⟩,|−⟩⊗|0⟩⊗|−⟩,|+⟩⊗|1⟩⊗|+⟩,|−⟩⊗|1⟩⊗|−⟩}$$Consider, the operators $P_1=Z⊗Z⊗I$ & $P_2=Z⊗I⊗Z$ both in $P_3$.
- $Z⊗Z⊗I$ stabilizes $4$ orthogonal states ${|000⟩,|001⟩,|110⟩,|111⟩}$.
- $Z⊗I⊗Z$ stabilizes $4$ orthogonal states ${|000⟩,|010⟩,|101⟩,|111⟩}$.

Intersection of these sets is ${|000⟩,|111⟩}$. This means that any state that is linear combo of these $2$ states will be stabilized by both $P_1$ & $P_2$.

In the above way you can find the simultaneous stabilizer states of any set of Pauli operators. Just find the individual stabilizer states of each operator. Then find the intersection of all the sets.
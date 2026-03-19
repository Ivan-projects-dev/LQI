#Algorithm #Math #Quantum 
We know that applying Hadamard to state $|0⟩$, we get $1√2(|0⟩+|1⟩)$, & applying Hadamard to state $|1⟩$, we obtain $1√2(|0⟩−|1⟩)$.

Now let $x_1∈{0,1}$. Consider the following expression. $H|x1⟩=1√2(|0⟩+(−1)x1|1⟩)=1√2∑z∈{0,1}(−1)x1z|z⟩$

When $x_1=0$, then we have a plus & when $x_1=1$, then we have minus. So, we can start expressing the state we get after applying Hadamard to arbitrary qubit using the above expression starting from now.

What happens when we apply the $H⊗n$ to an $n$-qubit register? We can intuitively guess that applying the operator to an all $0$ register would yield an equal superposition of all possible natural nums like $H⊗n|0⟩⊗n=1√2n2n−1∑x=0|x⟩$

What about arbitrary states? Then using the above equation we can write the following expression:
$$H⊗n∣x⟩=2n​1​z∈{0,1}n∑​(−1)x⋅z∣z⟩$$
where $|x⟩=|x_1⋯x_n⟩, |z⟩=|z_1⋯z_n⟩$ and x⋅z is the bitwise & operation modulo 2, i.e. $x⋅z=∑n_i=1x_i⋅z_i(mod2)$

We construct a circuit with $n+1$ [[Qubits]].
- Set the $n+1$'st qubit to state $|−⟩$ by applying $X$ and $H$ gates.
- Apply $H$ to first $n$ [[Qubits]].
- Apply $U_f$.
- Apply $H$ to first $n$ [[Qubits]].
- Measure the first $n$ [[Qubits]]. If it is $0_n$, then the func is const. Otherwise, it is balanced.

This time we have a circuit with $n$ input [[Qubits]] & an output qubit. The init state is $|ψ0⟩=|0⟩⊗n|0⟩$
Next we apply an $X$ gate to last qubit. $|ψ1⟩=|0⟩⊗n|1⟩$

We set last qubit to state |−⟩ and apply $H$ gate to first $n$ [[Qubits]]. $|ψ2⟩=1√2n2n−1∑x=0|x⟩⊗|−⟩$

Now we apply $U_f$. Recalling phase kickback, we can rewrite our entire state as $|ψ3⟩=[1√2n2n−1∑x=0(−1)f(x)|x⟩]⊗|−⟩$.

Note that we apply $U_f$ to each basis state & write our [[Quantum state]] using sum notation since we have an equal superposition of them.

Now we can ignore the output qubit & only focus on the input [[Qubits]] to write $|ψ3,0⟩=1√2n2n−1∑x=0(−1)f(x)|x⟩$

Now, we apply Hadamard to each qubit & we get the following result noting that $H⊗n|x⟩=1√2n∑2n−1z=0(−1)x⋅z|z⟩$.

$|ψ4,0⟩=12n2n−1∑x=02n−1∑z=0(−1)x⋅z+f(x)|z⟩$

We know the probability of observing state is equal to the square of its amplitude. Now let's focus on the state $z=|0⟩⊗n$. Replacing $z=0$ in the above sum, its amplitude is given by $12n2n−1∑x=0(−1)f(x)$.

So, if the func is const, then this sum adds up to $±1$, meaning that we observe $z=|0⟩⊗n 100$% of the time & the amplitudes of all other states cancel each other, resulting in $0$ probability.

Similarly, the probability of observing $z=|0⟩⊗n$ is $0$ if the func is **balanced** since exactly half of the terms have opposite signs & in this case we observe non-$0$ string.

In contrast to the classical strategy, which requires exponentially many queries, Deutsch-Jozsa can solve the same problem making only $1$ query. This is an **exponential separation**, with respect to [[Oracle]], that is in the [[Oracle]] model of computation.
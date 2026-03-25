#Quantum #Algorithm #Math 
Let $x⋅s$ represent the inner product of the bit strings modulo $2$. For instance if $x=1000$ and $s=1010$, then $$x⋅s=1⋅1+0⋅0+0⋅1+0⋅0=1(mod2)=1$$
Given an [[Oracle]] $f:{0,1}n→{0,1}$, which is defined as $f(x)=x⋅s$, find the secret string (sequence of bits) $s$.

This might come across as a bit of an artificially created problem, because it is. It was specifically designed to be solved using a quantum computer in order to show there can be advantages in using quantum algorithms over probabilistic algorithms.

Let's start by giving an example of such an $f$. $$f(00)=0f(01)=1f(10)=0f(11)=1$$ In this example, $s$ is $01$, as $$f(00)=00⋅01=0, f(01)=01⋅01=1, f(10)=10⋅01=0 & f(11)=11⋅01=1$$
Note that now the [[Unitary operator]] $U_f$ takes the following form: $$U_f:|x⟩|y⟩↦|x⟩|y⊕(x⋅s)⟩$$
We use exactly the same algorithm as [[Deutsch-Jozsa]].
![No description has been provided for this image](https://gitlab.com/qworld/qeducation/qbook101/raw/main/qbook101/images/ch4/deutschjozsa.png)

We construct a circuit with $n+1$ [[Qubits]].
- Set the $n+1$'st qubit to state $|−⟩$ by applying $X$ and $H$ gates.
- Apply $H$ to first $n$ [[Qubits]].
- Apply $U_f$.
- Apply $H$ to first $n$ [[Qubits]].
- Measure the first $n$ [[Qubits]] to obtain $s$.
![No description has been provided for this image](https://gitlab.com/qworld/qeducation/qbook101/raw/main/qbook101/images/ch4/deutschjozsa2.png)
As we have the same circuit as Deustch-Jozsa, the init is the same.
$$|ψ2⟩=1√2n2n−1∑x=0|x⟩⊗|−⟩$$
From now on we can ignore the output qubit & focus on our input [[Qubits]]. After applying $U_f$ we then get the state: $$|ψ3,0⟩=1√2n2n−1∑x=0(−1)f(x)|x⟩$$

Let's replace $f(x)=x⋅s$, & rewrite our state as follows: $$|ψ3,0⟩=1√2n2n−1∑x=0(−1)x⋅s|x⟩$$
We know $$H⊗n|x⟩=1√2n2n−1∑x=0(−1)x⋅z|z⟩$$, & the $H⊗n$ operator is its own inverse. Thus, we can say that $$H⊗n|a⟩=|b⟩⟺H⊗n|b⟩=|a⟩$$. So in fact, $|ψ3⟩$ is the state obtained after applying $H⊗n$ to $|s⟩$.

Hence after applying $H⊗n$ to the input [[Qubits]], we get the final state as $|ψ4,0⟩=|s⟩$.
We measure the first $n$ [[Qubits]] & we observe the string $s$ with probability $1$.
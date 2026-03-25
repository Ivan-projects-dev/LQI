#Algorithm #Quantum 
In $1992$ David Deutsch & Richard Jozsa dev a quantum algorithm that demonstrates, for the very first time, the potential of quantum computers to solve certain problems exponentially faster than classical computers.

Given a bool func $f:{0,1}→{0,1}$, we say $f$ is **balanced** if $f(0)≠f(1)$ and **const** if $f(0)=f(1)$.

Given $f:{0,1}→{0,1}$ as an [[Oracle]], that is we can evaluate it for an input by making queries but we can't look inside, the problem is to decide whether f is const or balanced.
![No descrovided for this image](https://gitlab.com/qworld/qeducation/qbook101/raw/main/qbook101/images/ch4/deutsch.png)
We construct a $2$ qubit circuit.
- Set the second qubit to state $|−⟩$ by applying $X$ and $H$ gates.
- Apply $H$ to first qubit.
- Apply $U_f$.
- Apply $H$ to first qubit.
- Measure the first qubit. If it is $0$ then $f$ is const. If it is $1$, then $f$ is balanced.
![No descr has been provided for this image](https://gitlab.com/qworld/qeducation/qbook101/raw/main/qbook101/images/ch4/deutsch_2.png)
We start with the init state $|ψ0⟩=|0⟩|0⟩$. Next we apply an $X$ gate to the second qubit & obtain the state $|ψ1⟩=|0⟩|1⟩$.
After applying $H$ to both [[Qubits]], the first qubit is in the equal superposition state & the second qubit is now in state $|−⟩$.
$|ψ2⟩=(1√2|0⟩+1√2|1⟩)|−⟩=1√2|0⟩|−⟩+1√2|1⟩|−⟩$

Next we apply $U_f$ to $|ψ2⟩$ and obtain $|ψ3⟩$
$$|ψ3⟩=Uf(1√2|0⟩|−⟩+1√2|1⟩|−⟩)=1√2Uf|0⟩|−⟩+1√2Uf|1⟩|−⟩$$
Linearity of the operator $= 1√2(−1)f(0)|0⟩|−⟩+1√2(−1)f(1)|1⟩|−⟩$
By phase kickback $=(1√2(−1)f(0)|0⟩+1√2(−1)f(1)|1⟩)|−⟩$

Let's focus on the first qubit. Now we will move on to [[Vector]] notation as the analysis will be easier. We can express $|ψ3⟩$ using the following [[Vector]]: $|ψ3,0⟩=1√2((−1)f(0)(−1)f(1))$

Next, we apply $H$ gate to first qubit & obtain the following state [[Vector]]:
$$ |ψ4,0⟩=1√2⎛⎜⎝1√21√21√2−1√2⎞⎟⎠((−1)f(0)(−1)f(1)) =12((−1)f(0)+(−1)f(1)(−1)f(0)−(−1)f(1)) {} {}$$

Now let's consider the $2$ cases.
- $f$ is const:
In this case $f(0)=f(1)$ and $|ψ4,0⟩=((−1)f(0)0)$ and the corresponding [[Quantum state]] is $|ψ4,0⟩=(−1)f(0)|0⟩$. Hence, we observe $0$ with probability $1$. (Since $f(0)=f(1)$, you can equivalently replace it.)
- $f$ is balanced:
In this case, $f(0)≠f(1)$ and $|ψ4,0⟩=(0(−1)f(0))$ and the corresponding [[Quantum state]] is $|ψ4,0⟩=(−1)f(0)|1⟩$. Hence, we observe $1$ with probability $1$.

So, we can find (with $100$% certainty) whether $f$ is const or balanced by making only a single query to func $f$.
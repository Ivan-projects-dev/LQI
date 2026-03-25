#Algorithm #Quantum #DB 
**[[Grover]] search** can search through an unsorted DB of $N$ items in approx $√N$ steps, compared to an $N$ num of steps for classical computer. [[Grover]] showcased another practical app of Quantum computing, reinforcing its potential to solve complex problems $>$ efficiently.

Given classical func, $f(x): {0,1}^n -> {0,1 }$, where $n$ is the bit-size of the search space, find an input $x_0$ for which $f(x_0)=1$.

Power of [[Grover]] search lies in its ability to amplify the probability of finding the correct solution through **quantum interference**. It uses a **quantum [[Oracle]]** to mark the desired item & then applies series of quantum operations to increase the amplitude of the marked state. This process, known as **amplitude amplification**, allows algorithm to find the target item with high probability after only $√N$ iterations. For large DBs, this quadratic Speedup can translate to significant time savings, making previously intractable search problems feasible.

We are going to use $n$ [[Qubits]]. at start we apply Hadamard to each qubit, so we put our [[Quantum state]] into superposition. The amplitude of each basis state $|0⋯0⟩,…,|1⋯1⟩$ is set to $1√N$. After that we iterate the following algorithm for several times:
- Make query: apply query [[Oracle]] operator to [[Qubits]] - it flips the sign of the amplitude of the state that corresponds to the marked element.
- Inversion: apply a diffusion [[Matrix]] - the amplitude of each state is reflected over the mean of all amplitudes.

To implement the **inversion/diffusion operation**, we will need additional (**[[Ancilla]]**) qubit. This is how we implement the inversion operator:
- Set the [[Ancilla]] qubit to $|−⟩$ by applying $X$ & $H$.
- Apply $H$ to all [[Qubits]] other than the [[Ancilla]].
- Apply $X$ to all [[Qubits]] other than the [[Ancilla]].
- Apply multiple controlled NOT operator, where the [[Ancilla]] qubit is target & all other [[Qubits]] are used for controlling.
- Apply $X$ to the [[Ancilla]] qubit.
- Apply $X$ to all [[Qubits]] other than the [[Ancilla]].
- Apply $H$ to all [[Qubits]] other than the [[Ancilla]].
- Set [[Ancilla]] qubit back by applying $X$ & $H$.

#Algorithm #Quantum 
Classical computers don't produce random nums, but rather pseudorandom nums based on some init value (**seed**), usually from CPU clock.

Quantum computers, on the other hand, can generate truly random nums, as measurement of qubit in superposition is probabilistic process. Result of the measurement is random, & there's no way to predict the outcome. This is the basic principle of quantum random num generators.

We start by taking a qubit in a basis state. First step of the random num generator is to use Hadamard operation to put the qubit into equal superposition. Measurement of this state results in $0$ or $1$ with $50$% probability of each outcome.

Let's say you repeat the process $4$ times, generating this sequence of binary digits: $0, 1, 1, 0$. After concatenation $0110 = 6$ in decimal. With $>$ repetitions, $>$ result num.

[[Logic]] of random num generator:
1. Define `max` as max num to be generated.
2. Define the num of random bits that need to generate. This is done by calc how many bits, `nBits`, you need to express ints up to `max`.
3. Generate random bit string that's `nBits` in length.
4. If the bit string represents num $>$ `max`, go back to step $3$.
5. Otherwise, the process is complete. Return generated num as int.

We need $floor((ln(max))/(ln(2)+1))$ bits to represent num between $0$ & $max$. We can use the built-in func `BitSizeI`, which takes any int & returns num of bits needed to represent it.

1. Start with qubit init in $|0〉$ state & apply `H` operation to create equal superposition in which the probabilities for $0$ & $1$ are same ([[Bloch sphere]] representation).
    
    ![A diagram showing the preparation of a qubit in superposition by applying the hadamard gate.](https://learn.microsoft.com/en-us/azure/quantum/media/qrng-h.png)
2. Then measure the qubit & save the output:
    
    ![A diagram showing the measurement of a qubit & saving the output.](https://learn.microsoft.com/en-us/azure/quantum/media/qrng-meas.png)
Since the outcome of measurement is random & probabilities of measuring $0$ & $1$ are same, you have obtained completely random bit. You can call this operation several times to create ints.
For example, if you call the operation $3$ times to obtain $3$ random bits, you can build random $3$-bit nums (that is, random num between $0$ & $7$).
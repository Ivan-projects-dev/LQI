#Q-Sharp #Quantum #Math
**Multi-qubit gates** act on $2>$ [[Qubits|qubits]] simultaneously. All listed here are in `Std.Intrinsic` & support `Adj + Ctl`.

**[[CNOT]] (Controlled-NOT) 
$$\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&0&1\\0&0&1&0\end{pmatrix}$$
Flips target qubit iff control is $|1\rangle$. Basis: $|00\rangle\to|00\rangle$, $|01\rangle\to|01\rangle$, $|10\rangle\to|11\rangle$, $|11\rangle\to|10\rangle$. Equivalent to `Controlled X([ctrl], target)`. 
```csharp
CNOT(ctrl, target);
// equivalent:
Controlled X([ctrl], target);
```

Used to create Bell states & entanglement:
```csharp
H(q0);
CNOT(q0, q1); // produces (|00⟩ + |11⟩)/√2
```

**CZ (Controlled-Z) - `CZ`**
$$CZ = \begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&1&0\\0&0&0&-1\end{pmatrix}$$
Applies $Z$ to target iff control is $|1\rangle$. **Symmetric** - control & target are interchangeable. $CZ^2 = I$.
```csharp
CZ(q0, q1);
// equivalent:
Controlled Z([q0], q1);
```

**SWAP - `SWAP`**
$$SWAP = \begin{pmatrix}1&0&0&0\\0&0&1&0\\0&1&0&0\\0&0&0&1\end{pmatrix}$$
Exchanges states of two [[Qubits]]: $|ab\rangle \mapsto |ba\rangle$. $SWAP^2 = I$. Decomposition: $SWAP = CNOT_{01} \cdot CNOT_{10} \cdot CNOT_{01}$.
```csharp
SWAP(q0, q1);
```
Used in QFT to reverse qubit order after the transform.

**CCNOT (Toffoli) - `CCNOT`**
$$CCNOT: |c_1 c_2 t\rangle \mapsto |c_1 c_2\,(t \oplus (c_1 \wedge c_2))\rangle$$
Flips target iff both controls are $|1\rangle$. Universal for reversible classical computation. Acts as $3$-qubit gate; $8\times8$ [[Matrix]] with $1$s on diagonal except last two rows swapped.
```csharp
CCNOT(ctrl1, ctrl2, target);
// equivalent:
Controlled X([ctrl1, ctrl2], target);
```
Key in [[Oracle]] construction - marks the all-$|1\rangle$ state in Diffusion operator.

**T-gate cost**: exact decomposition requires 7 T-gates. This makes CCNOT a significant resource on fault-tolerant hardware.


**CSWAP (Fredkin) - `Controlled SWAP`**
$$CSWAP: |c\,ab\rangle \mapsto \begin{cases}|c\,ab\rangle & c=0 \\ |c\,ba\rangle & c=1\end{cases}$$
Swaps $2$ [[Qubits]] controlled on $3rd$. No dedicated `CSWAP` keyword - use `Controlled SWAP`:
```csharp
Controlled SWAP([ctrl], (q0, q1));
```

Used in [[SWAP test]] for state comparison.


**Multi-controlled generalization**

Any single-qubit gate can be made $n$-controlled via the `Controlled` functor. Q# decomposes this automatically using ancilla & Toffoli gates:
```csharp
Controlled H([c0, c1, c2], target);      // 3-controlled H
Controlled T([c0, c1], target);          // 2-controlled T
```
Cost of $n$-controlled-$U$: $O(n)$ Toffoli gates using borrowed [[Ancilla qubit|ancilla]].

[[CNOT]], CZ, & SWAP are all maximally entangling for appropriate input states. Any $2$-qubit unitary can be decomposed into at most 3 [[CNOT]] gates + [[Single-qubit gates]]. This gives the **KAK decomposition** - the basis of most two-qubit gate compilers.
#Quantum #Math #Q-Sharp
**Ancilla qubit** (from Latin helper/servant) is auxiliary qubit allocated to assist computation without being part of the primary I/O register. Carry intermediate results, enable reversible computation, & facilitate phase kickback.

Quantum gates must be **reversible** (unitary). Classical [[Logic]] gates such as $AND, OR$ produce irreversible mappings - multiple inputs map to the same output, losing info. To implement $f(x)$ as unitary, extra register is introduced:
$$U_f : |x\rangle|a\rangle \mapsto |x\rangle|a \oplus f(x)\rangle$$
Input $|x\rangle$ is preserved; $f(x)$ is XOR-d into the ancilla $|a\rangle$. This makes the transform reversible regardless of $f$.

**Output ancilla**: initialized to $|0\rangle$, receives $f(x)$ after the [[Oracle]]. Used in marking oracles - the ancilla becomes $|1\rangle$ for solution states.

**Phase kickback ancilla**: initialized to $|{-}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$. When $U_f$ is applied, phase $(-1)^{f(x)}$ is kicked back to the input register; the ancilla returns unchanged. Used in all phase oracles ([[Oracle]]).

**Borrowed ancilla**: qubit temporarily "borrowed" from another part of the circuit, used during computation, then returned to its original state. Does not need to be $|0\rangle$ initially - computation must uncompute correctly regardless of borrowed state.

**Garbage ancilla**: ancilla left in state correlated with $|x\rangle$ after computation. Must be **uncomputed** (reset to $|0\rangle$) before release to avoid entanglement with the main register that would collapse superpositions upon measurement.

If an ancilla $|g(x)\rangle$ was produced as side-effect of computing $f(x)$, **uncomputing** - running the circuit in reverse ($U^\dagger$) to restore $|g(x)\rangle \to |0\rangle$:
$$|x\rangle|f(x)\rangle|g(x)\rangle \xrightarrow{U^\dagger_g} |x\rangle|f(x)\rangle|0\rangle$$
Without uncomputation, measuring $|f(x)\rangle$ collapses $|x\rangle$, destroying the superposition needed for quantum speedup. Within-Apply pattern in Q# enforces this automatically.
```csharp
use ancilla = Qubit(); // allocate single ancilla, starts at |0⟩
use ancillas = Qubit[n]; // allocate n ancilla qubits

// Must reset before release - Q# enforces this
Reset(ancilla);
ResetAll(ancillas);
```

`borrow` keyword allocates dirty (unknown state) ancilla - cheaper in some hardware contexts:
```csharp
borrow scratch = Qubit(); // borrowed dirty ancilla
// ... use scratch, then return it to its original state
```
Allocated [[Qubits]] go out of scope at the end of their `use` block; Q# runtime throws runtime error if any qubit is not in $|0\rangle$ on release.

Each ancilla is physical qubit on real hardware - scarce resource. [[Oracle]] construction for $m$-clause SAT formula requires $O(m)$ clause ancillas + 1 output ancilla. Minimizing ancilla count through in-place arithmetic or Bennett's pebbling strategies is active area of quantum circuit optimization.
#Algorithm #Quantum #Q-Sharp
Inputs required before running [[QPE]]: eigenstate $|u\rangle$ of the target unitary $U$, & peated apps of $U^{2^j}$.

**Eigenstates of $Z$, $S$, $T$ gates** – all $3$ share the same computational-basis eigenstates:

| Gate |      | Eigenvalue   |       |
| ---- | ---- | ------------ | ----- |
| $Z$  | $+1$ | $-1$         | $1/2$ |
| $S$  | $+1$ | $i$          | $1/4$ |
| $T$  | $+1$ | $e^{i\pi/4}$ | $1/8$ |

Prepare eigenstate $|0\rangle$ by doing nothing; prepare $|1\rangle$ by applying $X$.

**Unitary powers** – [[QPE]] requires controlled-$U^{2^j}$ for $j=0,1,\ldots,t-1$. $U^k$ is obtained by applying $U$ exactly $k$ times in sequence. Key property: $(U^a)^b = U^{ab}$, so powers compose.

In Q#, `OperationPow` from `Std.Canon` lifts any operation to its $k$-th power:
```csharp
import Std.Canon.*;

// Apply U^(2^j) controlled on phaseQubit
for j in 0..t-1 {
    let power = 1 <<< j;          // 2^j
    let UPow = OperationPow(U, power);
    Controlled UPow([phaseRegister[j]], eigenstate);
}
```
For large powers, prefer repeated squaring over calling `U` $2^j$ times naively — reduces circuit depth from $O(2^t)$ to $O(t)$ when $U^2$ can be expressed as a simpler circuit.

**Validating eigenstates** – to assert $|ψ\rangle$ is eigenstate of $U$: apply $U$ to $|ψ\rangle$, then assert the state is unchanged up to global phase (i.e., $U|ψ\rangle = e^{i\phi}|ψ\rangle$). In Q#: prepare state, apply $U$, apply $P^\dagger$ to map back to $|0\rangle$, assert $|0\rangle$.

**Single-bit PE** – distinguishes eigenvalues $+1$ vs $-1$ (phases $0$ vs $1/2$) using $1$ control qubit & $1$ call to controlled-$U$:
```
control: |0> ──H──●──H──M
eigenstate: |ψ> ──U──────
```
$+1$ eigenvalue → $H|0\rangle$ unaffected → measure $0$. $-1$ eigenvalue → phase kickback turns $|{+}\rangle$ into $|{-}\rangle$ → measure $1$.

**$2$-bit PE** – distinguishes eigenvalues $+1, i, -1, -i$ (phases $0, 1/4, 1/2, 3/4$):
- Run single-bit PE first: $+1$ & $i$ both measure $0$; $-1$ & $-i$ both measure $1$.
- If result was $0$: run again with a controlled-$U^2$ circuit (or add $S^\dagger$ rotation before second $H$) to distinguish $+1$ ($\varphi=0$) from $i$ ($\varphi=1/4$).
- If result was $1$: run again similarly to distinguish $-1$ ($\varphi=1/2$) from $-i$ ($\varphi=3/4$).

This is the building block of **iterative [[QPE]]** (see [[Iterative QPE]]), where each round extracts $1$ bit of $\varphi$ with a phase correction for previously known bits.

## Sources
- [QPE kata — eigenstate preparation exercises](https://quantum.microsoft.com/en-us/tools/quantum-katas)
- [GitHub: QPE eigenstate preparation samples](https://github.com/microsoft/qsharp/tree/main/samples/algorithms/iterative-phase-estimation)
- [GitHub: QPE samples](https://github.com/microsoft/qsharp/tree/main/samples/algorithms)

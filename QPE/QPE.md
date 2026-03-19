#Algorithm #Quantum #Math
**Quantum Phase Estimation (QPE)** solves: given unitary $U$ & $1$ of its eigenstates $|u\rangle$ satisfying $U|u\rangle = e^{2\pi i \varphi}|u\rangle$, estimate the phase $\varphi \in [0,1)$ to $t$ bits of precision.

$\varphi$ is unknown; QPE extracts it using inverse QFT on control register.

**Qubit layout**:
- $t$ **phase (control) [[Qubits]]** - store the estimate of $\varphi$ after inverse QFT. Precision: $\varphi$ accurate to $\pm 2^{-t}$ with high probability.
- $s$ **eigenstate [[Qubits]]** - hold $|u\rangle$ (the target register).

**Circuit - $3$ stages**:
1. **Superposition**: apply $H^{\otimes t}$ to all $t$ control [[Qubits]]:
$$|0\rangle^{\otimes t}|u\rangle \xrightarrow{H^{\otimes t}} \frac{1}{\sqrt{2^t}}\sum_{k=0}^{2^t-1}|k\rangle|u\rangle$$
2. **Controlled unitaries**: apply $C$-$U^{2^j}$ for $j = 0, 1, \ldots, t-1$, where the $jth$ control qubit controls $U^{2^j}$ on the target register:
$$\frac{1}{\sqrt{2^t}}\sum_{k=0}^{2^t-1}|k\rangle \cdot U^k|u\rangle = \frac{1}{\sqrt{2^t}}\sum_{k=0}^{2^t-1} e^{2\pi i \varphi k}|k\rangle|u\rangle$$
since $U^k|u\rangle = e^{2\pi i \varphi k}|u\rangle$. Target register is unchanged; phase is written into control register amplitudes.
3. **Inverse QFT**: apply $\text{QFT}^\dagger$ to the $t$ control [[Qubits]]:
$$\text{QFT}^\dagger\!\left(\frac{1}{\sqrt{2^t}}\sum_{k=0}^{2^t-1} e^{2\pi i \varphi k}|k\rangle\right) = |\tilde{\varphi}\rangle$$
where $|\tilde{\varphi}\rangle$ is sharply peaked at int $\tilde{\varphi} = \lfloor 2^t \varphi \rceil$ (nearest int to $2^t\varphi$).

**Measurement**: measuring control [[Qubits]] yields $\tilde{\varphi}$ with probability:

$$P(\tilde{\varphi}) = \frac{1}{2^{2t}}\left|\frac{\sin(\pi \cdot 2^t(\varphi - \tilde{\varphi}/2^t))}{\sin(\pi(\varphi - \tilde{\varphi}/2^t))}\right|^2$$

If $2^t\varphi$ is exact int: $P(\tilde{\varphi}) = 1$. Otherwise, the correct $t$-bit approximation is obtained with probability $\geq \frac{4}{\pi^2} \approx 0.405$ using $t$ [[Qubits]], or $\geq 1 - \epsilon$ using $t + \lceil\log_2(2 + \frac{1}{2\epsilon})\rceil$ [[Qubits]].

**Complexity**:

| Resource              | Cost |
|---|---|
| Control [[Qubits]]    | $t$ |
| Eigenstate [[Qubits]] | $s$ |
| $U$ apps              | $2^t - 1$ |
| QFT$^\dagger$ gates   | $O(t^2)$ |
| Total gates           | $O(2^t \cdot \text{cost}(U) + t^2)$ |

**Kitaev's [[Iterative QPE]]** reduces qubit count to $1$ control qubit by repeating $t$ rounds (bit by bit).
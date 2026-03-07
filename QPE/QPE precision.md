#Algorithm #Quantum #Math Precision & resource analysis for [[QPE]].
**Precision definition**: [[QPE]] with $t$ control [[Qubits]] estimates $\varphi$ to an **additive error** of at most $2^{-t}$:
$$|\hat{\varphi} - \varphi| \leq 2^{-t}$$
where $\hat{\varphi} = \tilde{\varphi}/2^t$ is the output & $\tilde{\varphi} \in \{0,1,\ldots,2^t-1\}$ is the measured int.

**Probability of success** with $t$ [[Qubits]]:
	If $2^t\varphi$ is an int: $P(\text{exact}) = 1$.
	Otherwise, let $\delta = 2^t\varphi - \lfloor 2^t\varphi \rfloor \in (0,1)$ be the fractional deviation. Probability of getting the nearest $t$-bit approximation: $$P(\text{nearest}) \geq \frac{4}{\pi^2} \approx 0.405$$
**Extra [[Qubits]] to boost success probability**: use $t + m$ [[Qubits]] & discard the $m$ least significant bits. Success probability of getting $\varphi$ correct to $t$ bits rises to:
$$P \geq 1 - \frac{1}{2(2^m - 1)}$$
In practice: $m = 3$ gives $P \geq 0.875$; $m = 5$ gives $P \geq 0.969$.

| Requirement | Control [[Qubits]] $t$ | $U$ applications |
|---|---|---|
| $n$ bits of precision | $n$ | $2^n - 1$ |
| Error $\leq \epsilon$ | $\lceil \log_2(1/\epsilon) \rceil$ | $O(1/\epsilon)$ |
| Success prob $\geq 1-\delta$ | $n + \lceil\log_2\frac{2}{\delta}\rceil$ | $O(1/\epsilon)$ |
**Standard quantum [[Limit]]** (classical / repetition-based): $N$ measurements give precision $O(1/\sqrt{N})$.

**Heisenberg [[Limit]]**: [[QPE]] achieves $O(1/N)$ precision using $N$ applications of $U$ - **quadratic improvement** in resource efficiency over classical approaches.

**Iterative [[QPE]] (Kitaev's method)**: uses only **1 control qubit** & $t$ separate rounds. Total $U$ calls: $2^t - 1$ - same as standard [[QPE]], but with min qubit overhead. Tradeoff: requires $t$ sequential rounds (no parallelism).

**Dominant error sources in practice**:
- **Trotterization error** (when $U = e^{-iHt}$ approximated): systematic phase error $\sim O(\Delta t^2)$ per Trotter step.
- **Finite coherence time**: decoherence during $O(2^t)$ gates - limits max usable $t$ on NISQ hardware.
- **State preparation error**: if $|\psi\rangle$ is not exact eigenstate, multiple phases appear with split probability.
- **Readout error**: mitigated by majority-vote repetition.

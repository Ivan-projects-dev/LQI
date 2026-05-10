#Algorithm #Math #Q-Sharp 
**[[Quantum counting]]** uses [[QPE]] on the [[Grover]] iterate $G$ to count num of solutions $M$ to a search problem over $N$ items - without enumerating them. It gives quadratic speedup over classical counting: $O(\sqrt{N/M})$ vs $O(N/M)$.
[[Grover]] iterate $G = -H^{\otimes n} Z_0 H^{\otimes n} \cdot O_f$ has eigenvalues $e^{\pm 2i\theta}$ where:
$$\sin^2\!\theta = \frac{M}{N}$$
[[QPE]] estimates the phase $\varphi = \theta / \pi$. From the measurement outcome $\tilde\varphi$:
$$\hat\theta = \pi\tilde\varphi \qquad \hat M = N \sin^2\!\hat\theta$$
**Precision**: with $t$ clock [[Qubits]], the estimate $|\hat M - M| \leq \epsilon N$ with $t = O(\log N + \log(1/\epsilon))$.
### Example: $4$-item search, $1$ solution
$N = 4$, $M = 1$: $\sin^2\theta = 1/4 \Rightarrow \theta = \pi/6$.
[[Grover]] eigenphase: $\varphi = \theta/\pi = 1/6 \approx 0.1\overline{6}$.
With $t = 3$ clock bits (resolution $1/8 = 0.125$), [[QPE]] returns $\tilde\varphi \in \{0/8, 1/8, 2/8, \ldots\}$.
Closest to $1/6$: outcome $1/8 \Rightarrow \hat\theta = \pi/8 \Rightarrow \hat M = 4\sin^2(\pi/8) \approx 0.59$ - rounds to $1$.
$>$ clock bits give better accuracy: $t=5$ gives $|\hat M - 1| < 0.2$ with high probability.
```csharp
import Std.Convert.*;
import Std.Math.*;
import Std.Measurement.*;
import Std.Arrays.*;
import Std.Canon.*;

// Oracle: marks |11⟩ in 2-qubit register (1 solution among 4)
operation MarkSolution(register : Qubit[], target : Qubit) : Unit is Adj + Ctl {
    Controlled X(register, target);   // fires on |11⟩ only
}

// Grover iterate: G = -H²·Z₀·H²·Oracle (phase version)
operation GroverIterate(register : Qubit[]) : Unit is Adj + Ctl {
    let n = Length(register);
    // Oracle as phase oracle: apply X to ancilla, kick phase
    use anc = Qubit();
    within { X(anc); H(anc); }
    apply  { MarkSolution(register, anc); }

    // Diffusion: -H^n · Z₀ · H^n
    ApplyToEach(H, register);
    within { ApplyToEach(X, register); }
    apply  {
        Controlled Z(register[1..n-1], register[0]);
    }
    ApplyToEach(H, register);
}

operation CountSolutions(nClock : Int, nSearch : Int) : Int {
    use (clock, search) = (Qubit[nClock], Qubit[nSearch]);

    // Prepare uniform superposition on search register
    ApplyToEach(H, search);

    // QPE on Grover iterate
    ApplyToEach(H, clock);
    for j in 0..nClock - 1 {
        let power = 1 <<< j;
        for _ in 1..power {
            Controlled GroverIterate([clock[j]], search);
        }
    }
    Adjoint ApplyQFT(clock);

    // Decode: phase φ → θ = πφ → M = N · sin²θ
    let bits = MResetEachZ(clock);
    let phaseInt = ResultArrayAsIntBE(bits);
    let phi = IntAsDouble(phaseInt) / IntAsDouble(1 <<< nClock);
    let theta = PI() * phi;
    let N = 1 <<< nSearch;
    let M = IntAsDouble(N) * Sin(theta)^2;

    ResetAll(search);
    return Round(M);
}

// CountSolutions(5, 2) → 1   (5 clock bits, 2 search qubits = N=4, M=1)
```
**Note**: for the degenerate case $M = 0$ or $M = N$, $\theta = 0$ & [[QPE]] returns $\tilde\varphi = 0$. $2$ eigenvalues $e^{+2i\theta}$ & $e^{-2i\theta}$ are symmetric - [[QPE]] may return either $\varphi$ or $1-\varphi$; take $\min(\tilde\varphi, 1-\tilde\varphi)$ before computing $\theta$.

[[Quantum counting]] is often used as **preprocessing step** to calibrate [[Grover]]'s algorithm when $M$ is unknown.
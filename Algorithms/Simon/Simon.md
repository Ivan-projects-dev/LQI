#Algorithm #Quantum
**Simon's problem**: given [[Oracle]] $f:\{0,1\}^n\to\{0,1\}^n$ that is either $1$-to-$1$ or $2$-to-$1$ with hidden period $s\neq 0^n$ such that $f(x)=f(y)\iff x=y\oplus s$, find $s$. Best classical algorithm needs $\Omega(2^{n/2})$ queries; Simon algorithm uses $O(n)$ - **provable exponential quantum speedup**.

**Quantum circuit** ($1$ run):
$$|0\rangle^n \xrightarrow{H^{\otimes n}} \frac{1}{\sqrt{2^n}}\sum_x|x\rangle \xrightarrow{U_f} \frac{1}{\sqrt{2}}\bigl(|x_0\rangle+|x_0\oplus s\rangle\bigr)|f(x_0)\rangle \xrightarrow{H^{\otimes n}} |z\rangle$$
After measuring $z$, we get random [[Vector]] satisfying $z\cdot s = 0 \pmod 2$.

**Steps**:
1. Prepare $n$ query [[Qubits]] in $|0\rangle^n$ & $n$ answer [[Qubits]] in $|0\rangle^n$.
2. Apply $H^{\otimes n}$ to query register → uniform superposition.
3. Apply [[Oracle]] $U_f$: $|x\rangle|0\rangle \to |x\rangle|f(x)\rangle$.
4. Measure answer register → collapses query to $\frac{1}{\sqrt{2}}(|x_0\rangle + |x_0\oplus s\rangle)$ for some $x_0$.
5. Apply $H^{\otimes n}$ to query register.
6. Measure query register → obtain $z$ with $z\cdot s=0\pmod 2$.

**Classical post-processing**: repeat quantum circuit $\approx n$ times to collect $n$ linearly independent vectors $z_1,\ldots,z_n$. Solve the binary linear system $\{z_i\cdot s=0\}$ over $\mathbb{F}_2$ to recover $s$ (Gaussian elimination). If $s=0^n$ (one-to-one), the system has only the trivial solution.

**Complexity**: $O(n)$ [[Oracle]] queries (quantum) vs $O(2^{n/2})$ classical. Num of quantum runs needed: $O(n)$ (probability of linear independence ≥ $1/4$ per run).

**Special case $s=0^n$**: $f$ is $1$-to-$1$. System only gives $z\cdot 0=0$ for any $z$, so all $2^n$ measurements are possible. No consistent non-$0$ $s$ can be extracted → conclude one-to-one.

#Algorithm #Quantum #Math
$2$ [[Oracle]] types appear in [[Grover's algorithm]]. Both encode the same bool func $f:\{0,1\}^n \to \{0,1\}$, but act differently on the quantum register.

**Marking/bit-flip [[Oracle]]** uses [[Ancilla]] qubit $|a⟩$. The [[Oracle]] XORs $f(x)$ into the [[Ancilla]]:
$$U_f^{\text{mark}} : |x⟩|a⟩ \mapsto |x⟩|a \oplus f(x)⟩$$
When $f(x)=1$, the [[Ancilla]] flips from $|0⟩$ to $|1⟩$, "marking" the solution state. The input register $|x⟩$ is unchanged.

**Phase/sign-flip [[Oracle]]**: no [[Ancilla]] output needed in final form. Applies conditional phase of $-1$ to marked states: $$U_f^{\text{phase}} : |x⟩ \mapsto (-1)^{f(x)}|x⟩$$Marked states gain $-1$ global phase relative to unmarked states, which [[Grover's algorithm]] exploits via amplitude amplification.

**Converting marking → phase [[Oracle]]** via phase kickback:
Set the [[Ancilla]] to $|{-}⟩ = \frac{1}{\sqrt{2}}(|0⟩ - |1⟩)$ before applying $U_f^{\text{mark}}$:
$$U_f^{\text{mark}}|x⟩|{-}⟩ = (-1)^{f(x)}|x⟩|{-}⟩$$
[[Ancilla]] returns to $|{-}⟩$ unchanged; the phase $(-1)^{f(x)}$ is kicked back to $|x⟩$. This is exactly $U_f^{\text{phase}}$

**Circuit recipe**:
- Apply $X$, then $H$ to [[Ancilla]] qubit → prepares $|{-}⟩$
- Apply $U_f^{\text{mark}}$
- Apply $H$, then $X$ to [[Ancilla]] → restores [[Ancilla]] to $|0⟩$
- Net effect on the input register is $U_f^{\text{phase}}$
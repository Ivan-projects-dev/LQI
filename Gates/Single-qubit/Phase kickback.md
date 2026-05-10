#Math #Algorithm 
**Phase kickback** is the mechanism by which [[Oracle]] transfers phase info from an [[Ancilla]] qubit back into the control register. It requires the [[Ancilla]] to be in $|{-}\rangle$.

If [[Oracle]] marks solution by flipping target qubit ($X$ on target when condition met), & the target is $|{-}\rangle = H|1\rangle$:
$$\text{O} \cdot |x\rangle|{-}\rangle = (-1)^{f(x)}|x\rangle|{-}\rangle$$
where $O$ - [[Oracle]]

$|{-}\rangle$ [[Ancilla]] is unchanged; the phase $(-1)^{f(x)}$ is **kicked back** onto $|x\rangle$. [[Grover]]'s [[Oracle]] & [[QPE]] both rely entirely on this mechanism.
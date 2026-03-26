#Math 
**Matrix** = rectangular array of nums 
$A = [a_{ij}],\quad B = [b_{ij}],\quad \alpha A + \beta B = [\alpha a_{ij} + \beta b_{ij}]$
$A + B = B + A$   $0 \cdot A = A \cdot 0 = 0$
$(s + t)A = sA + tA$
$tA = 0 \Rightarrow t = 0 \vee A = 0$
Elementary row operations:
1. **Interchanging** $2$ rows
2. **Adding** rows
3. **Row multiplication**

Let $A$ be $m \times n$, $B$ be $n \times p$. Then $AB$ is $m \times p$ with elements:  
$C_{ik} = \sum_{j=1}^n a_{ij} b_{jk}$ - **dot product form**

Each matrix is associated with a linear transformation:  
$A \in M_{n \times n}$  
$T_A: \mathbb{R}^n \to \mathbb{R}^n$, defined by $T_A(x) = Ax$ for all $x \in \mathbb{R}^n$
$T_A(sx + ty) = s T_A(x) + t T_A(y)$

**Identity matrix** contains only $1$ & $0$: $IA = AI = A$

A matrix $A \in M_{n \times n}$ is **invertible** if $\exists B \in M_{n \times n}$ such that:  
AB=In=BAAB = I_n = BA  
Otherwise: **singular**

**Inverses are unique:**  
$A^{-1}B = B A^{-1} = I$
If $A = \begin{bmatrix}a & b \\c & d\end{bmatrix}$ & $\det A = ad - bc \ne 0$ → $A$ is invertible, and:  $A^(-1)$ $= \dfrac{1}{[[AD]] - bc} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$
$\Delta = ad - bc$ ⟷ **determinant**

If **coefficient matrix** $A$ of a system $Ax = \vec{b}$ in $n$ vars is **invertible**, the system has a **unique solution**:  $x=$ $A^(-1)$ $\vec{b}$

**Matrix transposition**:
Let $A \in M_{m \times n}$  $A^T$ is the matrix obtained by **interchanging rows & columns** of $A$:  
$(AT)ij=Aji(A^T)_{ij} = A_{ji}$
$(A^T)^T = A$    
$(sA)^T = sA^T$
$(A + B)^T = A^T + B^T$
$(AB)^T = B^T A^T$
$(A^T)^n = (A^n)^T$    

 If $A^T = A$ → **symmetric matrix**
 If $A^T = -A$ → **skew-symmetric matrix**
**Cramer’s Rule:**
For 2-var system:$$
\begin{aligned}
ax + by = e \\
cx + dy = f
\end{aligned}
$$
$Δ=∣abcd∣,Δ1=∣ebfd∣,Δ2=∣aecf∣\Delta = \begin{vmatrix} a & b \\ c & d \end{vmatrix}, \quad \Delta_1 = \begin{vmatrix} e & b \\ f & d \end{vmatrix}, \quad \Delta_2 = \begin{vmatrix} a & e \\ c & f \end{vmatrix}$
Solution:  $x = \dfrac{\Delta_1}{\Delta}$, $y = \dfrac{\Delta_2}{\Delta}$
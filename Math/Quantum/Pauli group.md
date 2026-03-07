#Quantum #Math #ML 
Recall the Pauli matrices along with the identity [[Matrix]], ${I,X,Y,Z}$. These matrices have the property that multiplying them together in any way, gets us back to the same set of matrices up to a factor of $±1$ or $±i$.
For instance, $XY=iZ, YZ=iX, ZX=iY$.

There are one additional property of these matrices that we will use over & er again, & that is the _commutation properties_.
For $P, Q∈{I,X,Y,Z}$, we have either
- $P$ and $Q$ commute, i.e. $PQ=QP$, or
- $P$ and $Q$ anti-commute, i.e. $PQ=−QP$.
**Example:** $XY=−YX$, but $XI=IX$.

**Pauli group $P_1$**
The $16$ matrices, $P_1={±I,±iI,±X,±iX,±Y,±iY,±Z,±iZ}$,form a group. This means the elements of $P_1$ satisfy the group axioms:
- Multiplying any $2$ elements of $P_1$ gives us another element of $P_1$. This property is called **closure (under multiplication)** - equivalently, we say the set $P_1$ is **closed under multiplication**.
- Given $3$ elements $P,Q,R$ in the $P_1$, we have $(PQ)R=P(QR)$. This is true because the elements of $P_1$ are matrices. This property is called **associativity of multiplication**.
- There is an identity element, $I$, in $P_1$.
- For every element in $P_1$, its inverse is also inside $P_1$. For example, the inverse of $X$ is $X$ itself. The inverse of $iX$ is $−iX$.
Note that $P_1$ has $16$ elements, because there are $4$ Pauli matrices & $4$ possible phase factors $(±1,±i)$.

**Pauli group $P_2$**
Consider the [[Tensor]] product of $2$ Pauli matrices, meaning elements such as $X⊗Z$ or $Y⊗Z$.
Given $P=P(0)⊗P(1)$ and $Q=Q(0)⊗Q(1)$ in $P2,PQ=(P(0)⊗P(1))(Q(0)⊗Q(1))=P(0)Q(0)⊗P(1)Q(1)$.

**Example:** $(X⊗Z)(Y⊗Z)=(iZ)⊗I=i(Z⊗I)$
Suppose, we collect all such elements into a set. How many such elements are there? In $P(0)⊗P(1)$, the [[Matrix]] $P(0)$ can be one of $4$ Pauli matrices, & so can $P(1)$. Additionally, we have $4$ possible phase factors. In total, we will have $42+1=64$ elements in a set we will call $P_2$.

Again, we go over the $4$ axioms of groups, & make sure $P_2$ satisfies them.
- We have confirmed that multiplying these $64$ elements together in any way gives us one of the $64$ elements.
- Multiplication continues to be associative.
- The element $I⊗I$ is the identity element.
- We can easily construct the inverse of any element $P(0)⊗P(1)$ as $P(0)−1⊗P(1)−1$, & it will be one among the $64$ elements.

**Pauli group $P_n$**
It should be clear now, how to extend the above analysis to [[Tensor]] products of $n$ Pauli matrices, $P(0)⊗P(1)⊗⋯⊗P(n−1)$. By the same arguments as above, there will be $4n+1$ elements in $P_n$.

**Subgroup** is a subset of the elements of the group that itself form a group. Meaning the subset satisfies all $4$ of the axioms of the group.

**Theorem:** Let $H$ be a nonempty finite subset of a group $G$. Then $H$ is a subgroup of $G$ if $H$ is closed under multiplication. So, we only have to check the closure axiom, & if it holds so will the other $3$ axioms. This makes the task of checking if a subset is a subgroup much easier.
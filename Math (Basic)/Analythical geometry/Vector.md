#Math 
$|vecAB|$ - vector **length/magnitude**
**Collinear vectors** are parallel to same line & ve proportional coordinates.
**Coplanar vectors** are such if exist plane such that given vectors parallel to it.
$veca(a_1, a_2)$ $|veca| = sqrt(a_1^2 + a_2^2)$  $veca+-vecb =(a_1 +- b_1, a_2+-b_2)$
For $vecA(x_1, y_1)$ $vecB(x_2, y_2)$ $vecAB=(x_2-x_1, y_2-y_1)$
**Opposite vectors** have same magnitude but opposite direction.
$|vecx + vecy| <= |x| + |y|$  $k*veca=(ka_1, ka_2)$
$veca * alpha = vecb$ iff $vecb || veca$ $|vecb| / alpha = |veca|$ 
**Opposite vector:** $(-1)\vec{a} = -\vec{a}$
**Scalar multiplication:**  If $\vec{a}(a_1, a_2)$ & $\vec{b}(b_1, b_2)$, then:  $$\vec{a} \cdot \vec{b} = a_1 b_1 + a_2 b_2 = |\vec{a}||\vec{b}|\cos(\angle(\vec{a}, \vec{b}))$$
Given **n vectors** $\vec{a}_1, \dots, \vec{a}_n$, their **linear combo** is:
$$
\sum_{i=1}^{n} \alpha_ia_i
$$
The combo is **trivial** if $\forall i: \lambda_i = 0$  If $\exists i: \lambda_i \ne 0$, the combo is **non-trivial**
Vectors $\vec{a}_i$ are **linearly dependent** if there $\exists$ a **non-trivial linear combo** = $\vec{0}$ Otherwise - **linearly independent**
$T.$ 2 vectors are **collinear** iff they are **linearly dependent** (applies to **$3$ or $>$ vectors**)
$$\cos(\angle(\vec{a}, \vec{b})) = \dfrac{\vec{a} \cdot \vec{b}}{|\vec{a}||\vec{b}|}$$
If $BM$ is **median** of $\triangle ABC$, then: 
$$|BM| = \sqrt{\dfrac{AB^2 + BC^2}{2} - \dfrac{AC^2}{4}}$$
$\vec{a} = a_1 \vec{e}_1 + a_2 \vec{e}_2 + \dots$ - **components of vector** $\vec{a}$ in **basis** $\vec{e}_1, \vec{e}_2, \dots$
**Basis is orthogonal** if any 2 vectors are orthogonal.  
**Orthonormal** if it is orthogonal & each basis vector has length $1$
An **$n$-dimensional column vector** is $n \times 1$ [[Matrix|Matrix]]
- $\mathbb{R}^n$ = collection of all $n$-dimensional vectors over $\mathbb{R}$
- $\vec{a} = \begin{bmatrix} a_1 \ a_2 \ \dots \ a_n \end{bmatrix}$, $\vec{b} = \begin{bmatrix} b_1 \ b_2 \ \dots \ b_n \end{bmatrix}$
- $\vec{a} \cdot \vec{b} = \sum_{i=1}^n a_i b_i$

**Projection of vector** $\vec{a}$ **onto vector** $\vec{b}$ is projection of $\vec{a}$ onto any line collinear with $\vec{b}$:  
$\vec{a} = \mathrm{proj}_{\vec{e}_1}(\vec{a}) + \mathrm{proj}_{\vec{e}_2}(\vec{a}) = x\vec{e}_1 + y\vec{e}_2$  
$\vec{a} = \mathrm{proj}_{\vec{e}_1}(\vec{a}) + \mathrm{proj}_{\vec{e}_2}(\vec{a}) \Rightarrow \mathrm{proj}_{\vec{b}}(\vec{a}) = \dfrac{(\vec{a} \cdot \vec{b})}{|\vec{b}|^2} \vec{b}$
**Dot product:**  
$\vec{a} \cdot \vec{b} = |\vec{a}||\vec{b}|\cos(\angle(\vec{a}, \vec{b}))$  
$(\lambda \vec{a}) \cdot (\beta \vec{b}) = (\lambda \beta)(\vec{a} \cdot \vec{b})$  
$\vec{a} \cdot \vec{a} = |\vec{a}|^2$  
$\vec{a} \cdot \vec{b} = \vec{b} \cdot \vec{a}$  
$(\vec{a} + \vec{b}) \cdot \vec{c} = \vec{a} \cdot \vec{c} + \vec{b} \cdot \vec{c}$  
$\vec{a} \cdot \vec{b} = 0 \iff \vec{a} \perp \vec{b}$
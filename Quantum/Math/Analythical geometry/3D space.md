#Math 
**3D [[Cross product]]** of vectors $\vec{a}$ & $\vec{b}$ is [[Vector]] $\vec{c}$ such that:  
$\vec{c} = |\vec{a}||\vec{b}|\sin(\angle(\vec{a}, \vec{b}))$, $\vec{c} \perp \text{plane}(\vec{a}, \vec{b})$  
If $\vec{c} \ne \vec{0}$, then $\vec{a}, \vec{b}, \vec{c}$ form **right-handed basis** $\vec{c} = \vec{a} \times \vec{b}$

**Triple product** of vectors $\vec{a}, \vec{b}, \vec{c}$ is:  
$\vec{a} \cdot (\vec{b} \times \vec{c})$ - denoted $(\vec{a}, \vec{b}, \vec{c})$

**Volume of parallelepiped:**  
$V = |\vec{a} \cdot (\vec{b} \times \vec{c})| = |\vec{a}||\vec{b} \times \vec{c}|\cos(\angle(\vec{a}, \vec{b} \times \vec{c}))$
$V = 0 \iff \vec{a}, \vec{b}, \vec{c}$ are **coplanar**

$[\vec{a}, \vec{b}, \vec{c}] > 0$ **if** $\vec{a}, \vec{b}, \vec{c}$ **is right-handed basis**  
$[\vec{a}, \vec{b}, \vec{c}] < 0$ **is left-handed basis**

**4 points on same plane** $\Leftrightarrow$ **vectors are coplanar** $\Leftrightarrow$ **parallelepiped** $= 0$
$\vec{a} \cdot (\vec{b} \times \vec{c}) = \vec{b} \cdot (\vec{c} \times \vec{a}) = \vec{c} \cdot (\vec{a} \times \vec{b}) = -\vec{a} \cdot (\vec{c} \times \vec{b}) = -\vec{c} \cdot (\vec{b} \times \vec{a}) = -[\vec{b}, \vec{a}, \vec{c}]$  
**Rotation keeps the sign, each swap changes the sign**
$(\vec{a} + \vec{b}) \times \vec{c} = \vec{a} \times \vec{c} + \vec{b} \times \vec{c}$
$$\vec{a} \times \vec{b} = \begin{bmatrix} \vec{e_x} & \vec{e_y} & \vec{e_z} \\a_x & a_y & a_z \\b_x & b_y & b_z\end{bmatrix} = (\begin{bmatrix}a_y & a_z \\b_y & b_z\end{bmatrix}, - \begin{bmatrix}a_x & a_z \\b_x & b_z\end{bmatrix}, \begin{bmatrix}a_x & a_y \\b_x & b_y\end{bmatrix})$$
$$(\vec{a}, \vec{b}, \vec{c}) = (a_x, a_y, a_z) \cdot (\vec{b} \times \vec{c}) = \begin{bmatrix} a_x & a_y & a_z \\b_x & b_y & b_z \\c_x & c_y & c_z\end{bmatrix}$$
**Volume of parallelepiped:**  
$V_{\text{parallelepiped}} = |(\vec{a}, \vec{b}, \vec{c})| = | \begin{vmatrix} a_x & a_y & a_z \\b_x & b_y & b_z \\c_x & c_y & c_z\end{vmatrix} |$
**Volume of tetrahedron:** $\dfrac{1}{6} |(\vec{a}, \vec{b}, \vec{c})|$

A **3D surface** is described by **linear equation**:  $Ax + By + Cz + D = 0$, where $A^2 + B^2 + C^2 \ne 0$ If $A, B, C, D \ne 0$, the **general equation** becomes: $\dfrac{x}{a} + \dfrac{y}{b} + \dfrac{z}{c} = 1$ - **axis segment equation of a plane**.
#Math 
**Curve in plane** can be described by **func of 2 vars**: $f(x, y) = 0$  
**Surface in space** can be described by **func of 3 vars**: $f(x, y, z) = 0$
$x^2 - y^2 = 0 \Rightarrow (x + y)(x - y) = 0$
**Line/surface is algebraic** if $f(x, y, \dots)$ is a **polynomial in vars** $x, y, \dots$  
**Degree of line/surface** = **max degree** of $f(x, y, \dots)$
It is **the same in any affine [[Coordinate system]]**.
It’s **possible to represent lines or surfaces as intersection of multiple surfaces**
$\begin{cases} x = \alpha(t) \ y = \beta(t) \ z = \gamma(t) \end{cases}$ - **describes a line in space**, works for **arbitrary num  of dimensions**
**$\vec{r}_0$** - **radius [[Vector]] (point coordinate of fixed point on the line)**: $(x_0, y_0)$  
**$\vec{r}$** - **point coordinate of var point on the line**
$\vec{a}$ - **[[Vector]] collinear with the line** (**direction [[Vector]]**)  
$\vec{n}$ - **[[Vector]] orthogonal to the line** (**normal [[Vector]]**)
$\vec{r} = \vec{r}_0 + t \vec{a}$  
$\vec{r} - \vec{r}_0 = t \cdot \vec{a}$  
$\vec{r} - \vec{r}_0 \perp \vec{n}$
**Parametric equation of the line:** $\begin{cases} x = x_0 + t a_x \ y = y_0 + t a_y \end{cases}$
**To get point $(x, y)$**, we must take the **same $t$ in both equations**

**Points on the line** can be obtained by **taking** $t \in \mathbb{R}$ & **putting it into both equations**

If $(\vec{r} - \vec{r}_0) \cdot \vec{a} = 0$ - **canonical equation of line in determinant form**:  
$\begin{vmatrix}x-x_0 & y-y_0 \\a_x & a_y\end{vmatrix} = 0$
If $a_x \ne 0$ & $a_y \ne 0$  
We can get **canonical equation in the standard form**:  
$\dfrac{x - x_0}{a_x} = \dfrac{y - y_0}{a_y}$
If $(\vec{r} - \vec{r}_0) \cdot \vec{n} = 0$ - **general line equation**:  
$Ax + By + C = 0$, where $A^2 + B^2 \ne 0$
If $B \ne 0$, we can divide general equation by $B$:  
$y = kx + b$ - **slope-intercept equation of line**
$\begin{vmatrix}x & y-b \\1 & k\end{vmatrix} = 0$ - **line is collinear to [[Vector]]** $\vec{a} = (1, k)$

$k = \tan \varphi = \tan(\angle(\vec{e}_1, \vec{a}))$
If $A, B, C \ne 0$ - we get **axis segment equation of line**:  
$\dfrac{x}{a} + \dfrac{y}{b} = 1$
**Curve is interesting** if $\dfrac{\text{distance from } M \text{ to } F}{\text{distance from } M \text{ to } d} = \varepsilon = \text{const}$  ($\varepsilon > 0$, $M$ on curve)
There $∃$ **3 types of such curves**: **Parabola**, **Ellipse**, **Hyperbola**
These are the only possible **conic sections**

$\dfrac{MF}{MK} = \varepsilon = \text{const}$ - **eccentricity of curve**

$F$ - **focus**, $d$ - **directrix**  
**Focal param** = **distance between $F$ & $d$**

**In [[Coordinate system]]** where $F$ is **origin (center)** & directrix $d$ is **orthogonal to $x$-axis** & **resides on the left of $x$-axis**:  
$MK = |x + q|$  
$MF = \sqrt{x^2 + y^2}$

**init equation:**  
$x^2 + y^2 = \varepsilon^2 (x + q)^2$

- For $0 < \varepsilon < 1$ → **ellipse**
- For $\varepsilon = 1$ → **parabola**
- For $\varepsilon > 1$ → **hyperbola**  
    Where $\varepsilon$ is **eccentricity of the curve**
**Canonical equation of parabola:**  $y^2 = 2px$
**Distance between any point of parabola & any given line is the same** (by definition)

**Canonical equation of ellipse:** $\dfrac{x^2}{a^2} + \dfrac{y^2}{b^2} = 1$

Constants:  $a = \dfrac{r_1 + r_2}{2}$, $b = \sqrt{a^2 - c^2}$ - **semi-major** & **semi-minor axes**

$MF + M F' = \varepsilon_1 MK + \varepsilon_2 MK' = \varepsilon_1 |MK| + \varepsilon_2 |MK'| = 2a = \text{const}$

$\forall$ points, **sum of distances from this point to 2 given points is const**

If $c = \varepsilon a$ - **ellipse directrices**: $x = \pm \dfrac{a}{\varepsilon}$  
**Distance from ellipse to directrix**: $\left( \dfrac{x}{\varepsilon} - 1 \right) a$, where $\varepsilon = \dfrac{c}{a}$ $\sqrt{a^2 - \dfrac{c^2 a^2}{a^2}} = \sqrt{1 - \dfrac{c^2}{a^2}} = \dfrac{b^2}{a^2}$

So:  $x^2 + y^2 = b^2$
**When $a \rightarrow \infty$ & $b$ remains cons**, the **ellipse degenerates into 2 parallel lines** ($y = b$ & $y = -b$)

Let:  $a = \dfrac{c}{\sqrt{\varepsilon^2 - 1}}$,  $b = \sqrt{a^2 (\varepsilon^2 - 1)}$
**Canonical equation of hyperbola:**  
$\dfrac{x^2}{a^2} - \dfrac{y^2}{b^2} = 1$ - also has **2 directrices & foci**
$|MF - MF'| = \varepsilon |MK - MK'| = 2a$  
$\varepsilon \cdot 2d = 2a \Rightarrow d = \dfrac{a}{\varepsilon}$
**Linear eccentricity of hyperbola & ellipse** is distance between **its center & either of its 2 foci**: $c = \varepsilon a$, or $c = a \sqrt{\varepsilon^2 - 1}$

**Asymptote** - line such that the **distance between curve & line approaches $0$** as $1$ or both coordinates $\to \infty$
From hyperbola: $bx \pm ay = 0$
This **can degenerate into 2 lines** that intersect:  $\left( \dfrac{b}{a}x + y \right) \left( \dfrac{b}{a}x - y \right) = b^2$
**Given 2 points** $(x_1, y_1)$ & $(x_2, y_2)$ $\vec{a} = (x_2 - x_1,\ y_2 - y_1)$
$\begin{vmatrix}x-x_1 & y-y_1 \\x_2 - x_1 & y_2-y_1\end{vmatrix} = 0$
**Given arbitrary point** $R = (x, y) = \vec{r}$ & **line specified by** $\vec{r}_0 = (x_0, y_0)$ & $\vec{a} = (a_x, a_y)$
Let us **construct parallelogram**. **Distance** $d$ is **corresponding altitude** that can be computed via area $S$:  
$d = \dfrac{S}{|\vec{a}|} = \dfrac{1}{\sqrt{a_x^2 + a_y^2}} |a_x y - a_y x - x_0 a_y + y_0 a_x| = \dfrac{|A x + B y + C|}{\sqrt{A^2 + B^2}}$ - **formula for distance between arbitrary point** $(x, y)$ **and the line** $Ax + By + C = 0$
If $\vec{r} - \vec{r}_0 = t \cdot \vec{a}$ - **parametric equation of the line in space**:

$\begin{cases} x = x_0 + a_x t &  y = y_0 + a_y t & z = z_0 + a_z t \end{cases}$

If $a_x \ne 0$, $a_y \ne 0$, $a_z \ne 0$ - **we can express $t$ from each of 3 parametric equations**

$\dfrac{x - x_0}{a_x} = \dfrac{y - y_0}{a_y} = \dfrac{z - z_0}{a_z}$ - **canonical equations of a line in space**

$\vec{r} = \vec{r}_0 + t \vec{a}$ **means** that the **area of parallelogram is 0**, which means:  
$(\vec{r} - \vec{r}_0) \times \vec{a} = \vec{0}$

**Canonical equation in determinant form:**
$\begin{vmatrix}x-x_0 & y-y_0 \\a_x & a_y\end{vmatrix} = 0$
$\begin{vmatrix}x-x_0 & z-z_0 \\a_x & a_z\end{vmatrix} = 0$
$\begin{vmatrix}y-y_0 & z-z_0 \\a_y & a_z\end{vmatrix} = 0$

$\vec{r}_0 = (x_0, y_0, z_0)$ - **radius vector of fixed point on the plane**  
$\vec{r} = (x, y, z)$ - **radius vector of var point on the plane**

$\vec{a}_1, \vec{a}_2$ - **non-collinear vectors coplanar with plane**,  
$\vec{n}$ - **vector orthogonal to the plane**
$\vec{r} - \vec{r}_0 = u \vec{a}_1 + v \vec{a}_2$ - **parametric equation of the plane**

**Or:**  
$\vec{r} - \vec{r}_0 \perp \vec{n}$  
$(\vec{r} - \vec{r}_0, \vec{a}_1, \vec{a}_2)$ **are coplanar**  
$(\vec{r} - \vec{r}_0, \vec{a}_1, \vec{a}_2) = 0$

**Parametric equation of plane:** $\begin{cases} x = x_0 + u a_{1x} + v a_{2x} \ y = y_0 + u a_{1y} + v a_{2y} \ z = z_0 + u a_{1z} + v a_{2z} \end{cases}$

**Determinant equation of plane:** $\begin{vmatrix} x-x_0 & y-y_0 & z-z_0 \\a_1x & a_1y & a_1z \\a_2x & a_2y & a_2z\end{vmatrix} = 0$

Or using **normal vector**: $(\vec{r} - \vec{r}_0) \cdot \vec{n} = 0$
$x n_x + y n_y + z n_z = x_0 n_x + y_0 n_y + z_0 n_z$ $\Rightarrow n_x x + n_y y + n_z z = d$ - **general equation of a plane**

Let $M_1, M_2, M_3$ be **3 points not lying on the same line**

We have **2 vectors parallel to the plane**:  
$\vec{a}_1 = \overrightarrow{M_1 M_2} = (a_{1x}, a_{1y}, a_{1z}) = (x_2 - x_1, y_2 - y_1, z_2 - z_1)$  
$\vec{a}_2 = \overrightarrow{M_1 M_3} = (a_{2x}, a_{2y}, a_{2z}) = (x_3 - x_1, y_3 - y_1, z_3 - z_1)$

**Determinant form:**
$\begin{vmatrix} x-x_0 & y-y_0 & z-z_0 \\a_1x & a_1y & a_1z \\a_2x & a_2y & a_2z\end{vmatrix} = \begin{vmatrix} x-x_1 & y-y_1 & z-z_1 \\x_2 - x_1 & y_2 - y_1 & z_2 - z_1 \\x_3 - x_1 & y_3 - y_1 & z_3 - z_1\end{vmatrix} = 0$

**To write down equation of a plane through 2 given points**  
$M_1 = (x_1, y_1, z_1)$ & $M_2 = (x_2, y_2, z_2)$ & **parallel to vector** $\vec{a} = (a_x, a_y, a_z)$:

Let $\vec{a}_1 = \overrightarrow{M_1 M_2} = (x_2 - x_1,\ y_2 - y_1,\ z_2 - z_1)$

$\vec{r}$ - **radius vector of arbitrary point**  
$N(\vec{r})$ or $N(x, y, z)$ - **normal equation of line or plane**  
$\vec{a}$ - **vector collinear to line**

**Depending on the case**, we use formulas for **vector relations**. All 3 of:  
$\dfrac{a_x}{b_x} = \dfrac{a_y}{b_y} = \dfrac{a_z}{b_z}$  
Or: $\vec{a} \times \vec{b} = \vec{0}$  
Or: $\vec{a} \cdot \vec{b} = 0$

**init equation:** $x^2 + y^2 = \varepsilon^2 (x + q)^2$
- For $0 < \varepsilon < 1$ → **ellipse**
- For $\varepsilon = 1$ → **parabola**
- For $\varepsilon > 1$ → **hyperbola**  
    Where $\varepsilon$ is **eccentricity of the curve**
**Canonical equation of parabola:** $y^2 = 2px$
**Distance between any point of parabola & any given line is the same** (by definition)
**Canonical equation of ellipse:** $\dfrac{x^2}{a^2} + \dfrac{y^2}{b^2} = 1$
Constants: $a = \dfrac{r_1 + r_2}{2}$, $b = \sqrt{a^2 - c^2}$ - **semi-major** & **semi-minor axes**

$MF + M F' = \varepsilon_1 MK + \varepsilon_2 MK' = \varepsilon_1 |MK| + \varepsilon_2 |MK'| = 2a = \text{const}$
$\forall$ points, **sum of distances from this point to 2 given points is const**
**Points on the line** can be obtained by **taking** $t \in \mathbb{R}$ & **putting it into both equations**
If $(\vec{r} - \vec{r}_0) \cdot \vec{a} = 0$ - **canonical equation of line in determinant form**: $\begin{vmatrix} x - x_0 & y - y_0 \ a_x & a_y \end{vmatrix} = 0$ If $a_x \ne 0$ & $a_y \ne 0$  
We can get **canonical equation in the standard form**: $\dfrac{x - x_0}{a_x} = \dfrac{y - y_0}{a_y}$
If $(\vec{r} - \vec{r}_0) \cdot \vec{n} = 0$ - **general line equation**:  
$Ax + By + C = 0$, where $A^2 + B^2 \ne 0$
If $B \ne 0$, we can divide general equation by $B$:  
$y = kx + b$ - **slope-intercept equation of line**
$\begin{vmatrix} x & y - b \ 1 & k \end{vmatrix} = 0$ - **line is collinear to [[Vector]]** $\vec{a} = (1, k)$
$k = \tan \varphi = \tan(\angle(\vec{e}_1, \vec{a}))$
If $A, B, C \ne 0$ - we get **axis segment equation of line**:  $\dfrac{x}{a} + \dfrac{y}{b} = 1$
Let $M_1, M_2, M_3$ be **3 points not lying on the same line**
We have **2 vectors parallel to the plane**:  
$\vec{a}_1 = \overrightarrow{M_1 M_2} = (a_{1x}, a_{1y}, a_{1z}) = (x_2 - x_1, y_2 - y_1, z_2 - z_1)$  
$\vec{a}_2 = \overrightarrow{M_1 M_3} = (a_{2x}, a_{2y}, a_{2z}) = (x_3 - x_1, y_3 - y_1, z_3 - z_1)$
**Determinant form:** $\begin{bmatrix}x-x_0 & y-y_0 & z-z_0 \\a_1x & a_1y & a_1z \\a_2x & a_2y & a_2z\end{bmatrix} = \begin{bmatrix}x-x_1 & y-y_1 & z-z_1 \\x_2 - x_1 & y_2 - y_1 & z_2 - z_1 \\x_3 - x_1 & y_3 - y_1 & z_3 - z_1\end{bmatrix} = 0$
**To write down equation of a plane through 2 given points**  
$M_1 = (x_1, y_1, z_1)$ & _2 = (x_2, y_2, z_2)$ & **parallel to [[Vector]]** $\vec{a} = (a_x, a_y, a_z)$:
Let $\vec{a}_1 = \overrightarrow{M_1 M_2} = (x_2 - x_1,\ y_2 - y_1,\ z_2 - z_1)$ Then:
$\begin{bmatrix}x-x_1 & y-y_1 & z-z_1 \\x_2 - x_1 & y_2 - y_1 & z_2 - z_1 \\a_x & a_y & a_z\end{bmatrix} = 0$
$\begin{bmatrix}x-x_0 & y-y_0 & z-z_0 \\a_1x & a_1y & a_1z \\a_2x & a_2y & a_2z\end{bmatrix}$ = $\sqrt{ \begin{Vmatrix}a_1y & a_1z \\a_2y & a_2z\end{Vmatrix}^2 + \begin{Vmatrix}a_1x & a_1z \\a_2x & a_2z\end{Vmatrix}^2 + \begin{Vmatrix}a_1x & a_1y \\a_2x & a_2y\end{Vmatrix}^2}$

$\vec{r}$ - **radius [[Vector]] of arbitrary point**  
$N(\vec{r})$ or $N(x, y, z)$ - **normal equation of line or plane**  
$\vec{a}$ - **[[Vector]] collinear to line**
**Depending on the case**, we use formulas for [[Vector]] relations. All 3 of: $\dfrac{a_x}{b_x} = \dfrac{a_y}{b_y} = \dfrac{a_z}{b_z}$  
Or: $\vec{a} \times \vec{b} = \vec{0}$  
Or: $\vec{a} \cdot \vec{b} = 0$
**Polyhedron** - 3D shape with **flat polygonal faces**, **straight edges**, & **vertices**
**Dihedral angle** - **angle between 2 intersecting planes**
**Homogeneous systems:** If $Ax = 0$ & $A$ is invertible, then the system has only the **trivial solution** $x = 0$ (and **vice versa**)
**Polar form**: $x = r \cos \theta$, $y = r \sin \theta$, where $\theta$ is the **angle between $z$ & positive $x$-axis**. Unique **up to addition of $2\pi$ radians** $-\pi < \theta \leq \pi$
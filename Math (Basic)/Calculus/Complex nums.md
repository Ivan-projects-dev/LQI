#Math 
**Complex num** can be represented by a **[[Matrix]]**: $\begin{bmatrix}x & -y \\y & x\end{bmatrix}$
Where $x \in \mathbb{R}$ is the **real part**, $y \in \mathbb{R}$ is the **imaginary part**
**Real num**: $\begin{bmatrix}x & 0 \\0 & x\end{bmatrix}$
**Imaginary unit** $i$: $\begin{bmatrix}0 & -1 \\1 & 0\end{bmatrix}$
$i^2 = -1$
$z = x + yi$:
- $z_1 = z_2 \iff x_1 = x_2$ & $y_1 = y_2$
- $x + yi = 0 \iff x = 0$ & $y = 0$
$z_1 + z_2 = (x_1 + x_2) + (y_1 + y_2)i$
$z_1 - z_2 = (x_1 - x_2) + (y_1 - y_2)i$
$(a+bi)(c+di) = (ac - bd) + (ad + bc)i$
$\dfrac{a+bi}{c+di} = \dfrac{(a+bi)(c-di)}{c^2+d^2}$ (multiply by conjugate)
$z / z = 1$, $z = \bar{z} \iff z \in \mathbb{R}$

**Exponential & polar forms:**
$e^{i\theta} = \cos\theta + i\sin\theta$ ([[Euler's formulas]])
$e^{a+bi} = e^a\cos b + i\,e^a\sin b$
$r^{a+bi} = r^a\cos(b\ln r) + i\,r^a\sin(b\ln r)$

Polar $re^{i\theta}$ → Cartesian $a+bi$: $a = r\cos\theta$, $b = r\sin\theta$
Cartesian $a+bi$ → Polar: $r = \sqrt{a^2+b^2}$, $\theta = \arctan\!\left(\dfrac{b}{a}\right)$

**Modulus:**  
$|z| = \sqrt{x^2 + y^2}$  
$|z_1 - z_2| = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}$
  
$|z_1 z_2| = |z_1||z_2|$,  
$\overline{z_1 z_2} = \bar{z}_1 \bar{z}_2$,  
$\overline{z_1 / z_2} = \bar{z}_1 / \bar{z}_2$

**Principal argument:**
$\mathrm{Arg},z \in (-\pi, \pi]$  
If $\theta_1$ & $\theta_2$ are arguments of $z_1, z_2$ →  
$arg⁡(z1z2)=θ1+θ2\arg(z_1 z_2) = \theta_1 + \theta_2$

If $\Theta$ is **argument of** $z$, then $n\Theta$ is **argument of** $z^n$
- $\arg(z_1 z_2) = \arg z_1 + \arg z_2 + 2\pi k_1$
- $\arg(\bar{z}) = -\arg z + 2\pi k_2$
- $\arg\left(\dfrac{z_1}{z_2}\right) = \arg z_1 - \arg z_2 + 2\pi k_3$
- $\arg(z^n) = n \arg z + 2\pi k_4$
Where $k_1, k_2, k_3, k_4 \in \mathbb{Z}$ are chosen to keep result in range $(-\pi, \pi]$

If $\theta_1$ & $\theta_2$ are **arguments of** $z_1$ & $z_2$, then $\theta_1 - \theta_2$ is **argument of** $\dfrac{z_1}{z_2}$

**De Moivre’s $T.$**
If $n \in \mathbb{Z}^+$:  $(cos⁡Θ+isin⁡Θ)n=cos⁡nΘ+isin⁡nΘ(\cos \Theta + i \sin \Theta)^n = \cos n\Theta + i \sin n\Theta$

**Hyperbolic identities:**
- $i = e^{\frac{\pi i}{2}}$
- $\cos\left(\dfrac{\pi}{2}\right) = 0$
- $\sinh\left(\dfrac{\pi}{2}\right) = i$
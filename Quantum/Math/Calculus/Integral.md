#Math  
$\int x^n dx = \dfrac{x^{n+1}}{n+1} + C$    
$\int dx = x + C$
$\int \sin x,dx = -\cos x + C$
$\int \cos x,dx = \sin x + C$
$\int \dfrac{1}{x} dx = \ln |x| + C$, $x > 0$
$\int \dfrac{1}{1 + x^2} dx = \tan^{-1} x + C$
$\int \dfrac{1}{\sqrt{a^2 - x^2}} dx = \sin^{-1} \dfrac{x}{a} + C$

$\int_a^b kf(x),dx = k \int_a^b f(x),dx$
$\int_a^b (f(x) + g(x)),dx = \int_a^b f(x),dx + \int_a^b g(x),dx$
$\int_a^b f(g(x)) g'(x),dx = \int_{g(a)}^{g(b)} f(u),du$
If $a > b$, then $\int_a^b f(x),dx = -\int_b^a f(x),dx$
- $\int_a^c f(x),dx = \int_a^b f(x),dx + \int_b^c f(x),dx$
- $\int_a^b k f(x),dx = k \int_a^b f(x),dx$
- $\int_a^b \left(f(x) \pm g(x)\right),dx = \int_a^b f(x),dx \pm \int_a^b g(x),dx$
- $\int_a^b f'(x),dx = f(b) - f(a)$

**Substitution rule:**
- $\int f(g(x)) g'(x),dx = f(g(x)) + C$
- $\int_a^b f(g(x))g'(x),dx = \int_{g(a)}^{g(b)} f(u),du$, $u = g(x)$

If $f$ is even: $\int_{-a}^{a} f(x),dx = 2 \int_0^a f(x),dx$
If $f$ is odd: $\int_{-a}^{a} f(x),dx = 0$
**Standard integrals:**
- $\int \dfrac{1}{\sqrt{a^2 - u^2}},du = \sin^{-1} \left( \dfrac{u}{a} \right) + C$
- $\int \dfrac{du}{u \sqrt{u^2 - a^2}} = \dfrac{1}{a} \sec^{-1} \left| \dfrac{u}{a} \right| + C$
- $\int \dfrac{du}{a^2 + u^2} = \dfrac{1}{a} \tan^{-1} \left( \dfrac{u}{a} \right) + C$
**Integration by parts**
- $\int u,dv = uv - \int v,du$
- $\int_a^b u(x) v'(x),dx = u(x)v(x) \big|_a^b - \int_a^b v(x)u'(x),dx$
**$>$ standard forms:** $\int \sqrt{a^2 - u^2},du = \dfrac{u}{2} \sqrt{a^2 - u^2} + \dfrac{a^2}{2} \sin^{-1} \left( \dfrac{u}{a} \right) + C$
**Area under semicircle**: $\int \sqrt{a^2 - u^2},du = \dfrac{u}{2} \sqrt{a^2 - u^2} + \dfrac{a^2}{2} \sin^{-1} \left( \dfrac{u}{a} \right) + C$
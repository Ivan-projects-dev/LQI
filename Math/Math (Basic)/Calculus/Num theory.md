#Math #CPP 
**$a \equiv b \ (\text{mod} \ m)$** means $m \mid (a - b)$  
Equivalent to: $a \mod m = b \mod m$, or $a = b + km$
If $a \equiv b \ (\text{mod} \ m)$ &  \equiv d \ (\text{mod} \ m)$:
- $a + c \equiv b + d \ (\text{mod} \ m)$
- $a \cdot c \equiv b \cdot d \ (\text{mod} \ m)$
- $(a + b) \mod m = (a \mod m + b \mod m) \mod m$
- $(a \cdot b) \mod m = (a \mod m \cdot b \mod m) \mod m$
**Linear congruence:** $ax \equiv b \pmod{m}$, solve for **$x$**
**Pseudoprimes:** $2^{n-1} \equiv 1 \pmod{n}$
**Hexagon identity:** $\binom{n - 1}{k - 1} \binom{n}{k + 1} = \binom{n + 1}{k} \binom{n - 1}{k}$
**Pascal’s identity:** $(n+1k)=(nk)+(nk−1)\binom{n + 1}{k} = \binom{n}{k} + \binom{n}{k - 1}$
**Binomial $T$**: $(x+y)n=∑k=0n(nk)xn−kyk(x + y)^n = \sum_{k=0}^{n} \binom{n}{k} x^{n - k} y^k$
$∑k=0n(nk)=2n\sum_{k=0}^n \binom{n}{k} = 2^n$
$∑k=0n(nk)2=(2nn)\sum_{k=0}^n \binom{n}{k}^2 = \binom{2n}{n}$
$∑k=0nk⋅(nk)=n⋅2n−1\sum_{k=0}^{n} k \cdot \binom{n}{k} = n \cdot 2^{n - 1}$
**Permutation:** $P(n, r) = \dfrac{n!}{(n - r)!}$  
**combo:** $\binom{n}{r} = \dfrac{n!}{r!(n - r)!}$
$\prod_{i=1}^n a_i$ - **product** of elements $a_1 a_2 \dots a_n$

**Canonical form** of polynomial: $p(x) = a_n x^n + a_{n-1} x^{n-1} + \dots + a_1 x + a_0$  
**Degree** = $n$
- If $\deg p < \deg q$ then $\deg(p + q) = \deg q$
- If $\deg p = \deg q$ then $\deg(p + q) \le \deg q$
**Degree of polynomials**: If $\deg(pq) = \deg p + \deg q$, then $pq \ne 0$
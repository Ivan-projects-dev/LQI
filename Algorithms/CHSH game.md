#Algorithm #Quantum
**Nonlocal game** between $2$ cooperating players (Alice & Bob) & referee. Players cannot communicate after the game starts but may share resources prepared beforehand.

**Setup**: referee gives Alice bit $X$ & b bit $Y$ independently & uniformly from $\{0,1\}^2$. Alice returns $A$, Bob returns $B$.
**Win condition**: $X\land Y = A\oplus B$
i.e., outputs must differ ($A\neq B$) iff both inputs are $1$; otherwise outputs must agree.

**Classical strategy** - best achievable win rate is $\mathbf{75\%}$:

| $(X,Y)$ | Required $A\oplus B$ |
|---------|----------------------|
| $(0,0)$ | $0$ (same) |
| $(0,1)$ | $0$ (same) |
| $(1,0)$ | $0$ (same) |
| $(1,1)$ | $1$ (different) |

Any deterministic strategy satisfies $3/4$ cases. Randomizing doesn't help - expected value over deterministic strategies is still $\leq 3/4$.

**Quantum strategy** - win rate $\cos^2(\pi/8) \approx \mathbf{85.4\%}$:

Before the game, Alice & Bob share a [[Bell states|Bell pair]] $|\Phi^+\rangle = \frac{|00\rangle+|11\rangle}{\sqrt{2}}$.

- **Alice**: if $X=0$, measure in $Z$ basis; if $X=1$, measure in $X$ basis. Return result as $A$.
- **Bob**: if $Y=0$, rotate qubit by $+\pi/8$ around $Y$-axis then measure in $Z$ basis; if $Y=1$, rotate by $-\pi/8$ then measure. Return result as $B$.

The $\pi/8$ angle is optimal: it is chosen so that the measurement bases are separated by $\pi/4$, maximising the correlation overlap via $\cos^2(\pi/8)$.

**Why this matters**: the $85.4\% > 75\%$ classical bound demonstrates **quantum nonlocality** - the shared entanglement provides correlations that no pre-shared classical strategy can replicate. This is a direct demonstration of violation of Bell's inequality 

(CHSH variant: $\langle AB\rangle-\langle AB'\rangle+\langle A'B\rangle+\langle A'B'\rangle \leq 2$ classically; quantum achieves $2\sqrt{2}$).

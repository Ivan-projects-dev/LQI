#SoftDev #Math #Hardware 
[[D-Wave]] solves problems by finding the min-energy state of [[QUBO]] energy func. Any constraint on the solution must be encoded as penalty term - sub-expression that has low energy when the constraint is satisfied & high energy when violated. This is the central [[D-Wave]] programming skill.
### Equality Constraint → Penalty Term
To enforce `x0 + x1 = 1`, rearrange to `x0 + x1 - 1 = 0`, square it, multiply by penalty weight $M$:
$$P = M \cdot (x_0 + x_1 - 1)^2$$
Expanding (dropping the constant $+1$):
$$P = M \cdot (-x_0 - x_1 + 2 x_0 x_1)$$
Which gives [[QUBO]] coefficients: `Q[(0,0)] = -M`, `Q[(1,1)] = -M`, `Q[(0,1)] = 2M`.

General rule: **move all terms to one side, square, multiply by $M$.**
### 1-Hot (Exactly-1-of-N) Constraint
"Exactly one of $n$ variables equals $1$" is the most common constraint in combinatorial problems (graph coloring, assignment, routing):
$$P = M \cdot \left(\sum_{i=1}^n x_i - 1\right)^2$$
Coefficients: diagonal $= -M$, every off-diagonal pair $= +2M$. Ocean has generator:
```python
import dimod
bqm = dimod.generators.combinations(['x0', 'x1', 'x2', 'x3'], 1)
# minimized when exactly 1 of the 4 variables = 1
```
For "exactly $k$ of $n$": replace $1$ with $k$ in the formula.
### Inequality Constraint → Slack Variable
`x0 + x1 <= 1` cannot be squared directly. Convert to equality using binary slack variable $a$:
$$x_0 + x_1 + a = 1 \quad \Rightarrow \quad P = M \cdot (x_0 + x_1 + a - 1)^2$$
$a = 0$: constraint tight ($x_0 + x_1 = 1$). $a = 1$: slack used ($x_0 + x_1 = 0$). Both are valid.
```python
Q = {(0,0): -1, (1,1): -1, ('a','a'): -1,
     (0,1): 2, (0,'a'): 2, (1,'a'): 2}
```
For larger slack ranges (e.g. `x0 + x1 <= 3`), use binary expansion: $a = a_0 + 2a_1$ (adds $2$ [[Ancilla]] bits).
### Penalty Weight Selection
**Penalty weight $M$ must exceed the max objective gain from violating the constraint.** Rule of thumb: set $M = 1.5 \times$ (largest coefficient in the objective). Ocean's scaling helper:
```python
bqm_constraint.scale(1.5)
combined = bqm_objective + bqm_constraint
```
Too small: infeasible solutions win. Too large: the objective signal is drowned out & the annealer just finds min-violation states regardless of solution quality.
### Verify Before Submitting to QPU
For small instances ($n \le 4$), enumerate all $2^n$ states & confirm the lowest-energy state is the correct solution:
```python
for bits in itertools.product([0, 1], repeat=n):
    sample = dict(zip(variables, bits))
    energy = sum(Q.get((i, j), 0) * sample[i] * sample[j]
                 for i in variables for j in variables)
    if is_valid(sample):
        print(f"{sample}: E={energy:.2f}")
```
If the lowest valid energy is not lower than any invalid energy, the penalty weight is too small. Fix the weight before spending QPU time.
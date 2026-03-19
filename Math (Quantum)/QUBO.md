#Quantum #Math 
**Quadratic Unconstrained Binary Optimization (QUBO) problem** is defined using an $n×n$ [[Matrix]] $Q$ and a [[Vector]] $x∈{0,1}n$ where,
- Q is assumed to be either symmetric or in upper-triangular form. For ease of use, we will work with Q in its upper-triangular form.
- x is a [[Vector]] of binary vars $0$ and $1$

Our aim is to minimize the **objective func** defined as $f(x)=∑_iQ_i,ix_i+∑i<jQ_i,jx_ix_j$
where,
- $Q_(i, i)$ terms are the **linear** coefficients of the func,
- $Q_(i, j)$ terms are the **quadratic** coefficients of the func.
An objective func gives a math description of a problem. We should minimize this objective func to find an optimal solution to our problem. In most cases, the lower the value of the objective func, the better our obtained solution is. The above objective func can be equivalently expressed as $min x∈{0,1} nxTQx$.

QUBO formulation is not just limited to minimization problems. It can be used for maximization problems too! To find an optimal solution to a maximization problem, we have to minimize the negative of its objective func:
$max_x∈{0,1}n_xTQ_x=min_x∈{0,1}n− xTQ_x$.
In QUBO formulation, we have a product of at most ${} 2 {}$ terms, the vars are binary & there are no constraints (constraints in general are in the form of linear inequalities over $x_i$).

Cost func $f(x)=−(∑i,jx_i+x_j−2x_ix_j)$ through binary vars $x_i$ such that
- Node $i$ belongs to group $0$ if $x_i=0$,
- Node $i$ belongs to group $1$ if $x_i=1$.
This is exactly a QUBO formulation for the Max-Cut problem.
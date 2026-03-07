#ML #Math #DB
Pick best attrs & conditions on which to partition. 
Purity of a set $S$ of training instances can be measured quantitatively in several ways.
num of classes - $k$, num of instances - $|S|$, fraction of instances in class $i = p_i$. 
**Gini measure of purity** is defined as Gini $(S) = 1 - ∑^k _(i-1) p^2 i$
When all instances are in a single class, the Gini value is $0$
It reaches its max of $1 – (1/k)$ if each class the same num of instances.
Another measure of purity is the entropy measure, which is defined as $k$ 
entropy $(S) = – ∑^k_(i- 1) p_i log_2 p_i$ 
When a set $S$ is split into multiple sets $S_i, I=1, 2, …, r$, we can measure the purity of the resultant set of sets as:
$purity (S_1, S_2, ….., S_r) = ∑^r_(i=1) |S_i|/|S| purity ( S_i)$
The info gain due to particular split of $S$ into $S_i, i = 1, 2, …., r$
**Info-gain** $(S, {S_1, S_2, …., S_r) = purity(S) – purity (S_1, S_2, … S_r)$
Measure of “cost” of a split: **Info-content** $(S, {S_1, S_2, ….., S_r})) = – ∑^r_(i-1) |S_i|/|S| log_2|S_i|/|S|$
Info-gain ratio = Info-gain / Info-content 
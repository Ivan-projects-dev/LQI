#DB #ML #Math 
Bayesian classifiers use Bayes theorem, which says $$p (c_j | d) = (p (d | c_j) p(c_j)) / (p (d))$$where $p (c_j | d)$ - probability of instance $d$ being in class $c_j$, 
	$p (d | c_j)$ - probability of generating instance $d$ given class $c_j$,
	$p (c_j)$ - probability of occurrence of class $c_j$,
	$p (d)$ - probability of instance $d$ occurring.
**Bayesian classifiers** require 
	• computation of $p (d | c_j)$ 
	• precomputation of $p (c_j)$ 
	• $p (d)$ can be ignored since it is the same for all classes 
To simplify the task, **naïve Bayesian classifiers** assume attrs have independent distributions, & thereby estimate $p (d | c_j) = p (d_1 | c_j) * p (d_2 | c_j) * ….* (p (d_n | c_j)$
• Each of the $p (d_i | c_j)$ can be estimated from a histogram on di values for each class $c_j$ - the histogram is computed from the training instances.
Histograms on multiple attrs are $>$ expensive to compute & store.
#Q-Sharp #Algorithm #Quantum
Index of all oracle implementations in the vault.

## Oracle Theory
- [[Oracle]] - mathematical definition, $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$
- [[Phase vs marking oracle]] - converting between types; phase kickback derivation

## Core Patterns
- [[Grover Oracle in Q-Sharp]] - marking oracle signature, `ControlledOnInt`, multi-solution, SAT structure
- [[Oracle Composition Patterns]] - `within/apply`, AND/OR/NOT of oracles, factory pattern, phase kickback wrapper, $U^{2^k}$

## Boolean Function Oracles
- [[Boolean Function Oracles]] - single-bit check, AND, OR, XOR, parity, majority, threshold (at-least-$k$)

## Arithmetic Oracles
- [[Arithmetic Oracles]] - equality, inequality, greater-than, range, odd/even, divisibility, weighted sum threshold

## Algorithm-Specific Oracles
- [[Grover oracles]] - AllOnes, AlternatingBits, ArbitraryPattern
- [[SAT oracle]] - CNF clause oracle, clause combination
- [[Graph coloring oracle]] - edge equality check, valid coloring mark
- [[Bounded knapsack oracle]] - weight+value threshold, Grover optimization loop
- [[Simon oracle]] - CountBits, BitwiseRightShift, LinearOperator, MultidimensionalLinearOperator
- [[QPE Oracle]] - controlled-$U^{2^k}$, `OperationPow`, repeated squaring, iterative QPE variant

## Oracles Used in Specific Algorithms
- [[Bernstein-Vazirani in Q-Sharp]] - inner product oracle: CNOTs for each set bit of $s$
- [[Deutsch-Jozsa in Q-Sharp]] - constant-0, constant-1, balanced parity, balanced bitstring

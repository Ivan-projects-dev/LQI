#ADT 
**$T$-gates** are non-Clifford & cannot be directly fault-tolerantly executed - they must be *distilled* from noisy T-states via magic state distillation. Estimator models $T$-factories as separate qubit blocks running in parallel:
- $>$ $T$-factory copies $→$ $>$ physical [[Qubits]], but $<$ runtime ($T$-states produced faster)
- $<$ $T$-factory copies $→$ $<$ [[Qubits]], but $>$ runtime (algorithm idles waiting for $T$-states)

This is the core **space-time tradeoff**: the Pareto frontier of (physical [[Qubits]], runtime) pairs is plotted as monotonically decreasing curve.

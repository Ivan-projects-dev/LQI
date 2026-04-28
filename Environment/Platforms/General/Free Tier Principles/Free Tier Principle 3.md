#SoftDev **Use Fewer Shots on Real Hardware**

Simulation shots are free. QPU shots cost money & time. You do not need $8192$ shots on every circuit.

Rules of thumb:
- For qualitative checks (is this the right distribution shape?): $256$-$512$ shots
- For quantitative error rate measurement: $1024$ shots
- For high-precision estimation or rare event detection: $4096$+ shots

A Bell state at $256$ shots still clearly shows the `00`/`11` pattern. You do not need $4096$ shots to confirm that.

For [[IBM Quantum]] free tier: $1024$ shots per circuit is the default &  adequate for most educational experiments.

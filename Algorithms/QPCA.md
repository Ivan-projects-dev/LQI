#Algorithm #Physics #Chemistry #ML
**QPCA** is a quantum algorithm that applies [[QPE]] to the data covariance [[Matrix]] (represented as a density [[Matrix]]) to extract eigenvalues & eigenvectors - the principal components. It comes out of Classical PCA, which identifies directions of max variance & projects data onto principal components for dimensionality reduction. Classical PCA is bottlenecked by [[Matrix]] diagonalization, which scales as $O(d^3)$ for $d$-dimensional data.

QPCA was proposed by Lloyd, Mohseni & Rebentrost (2014) and, under specific assumptions, offers an exponential reduction in query complexity. **These assumptions are restrictive:**
- Data must already be available as a quantum density [[Matrix]] (or loaded via [[Amplitude Encoding]] from QRAM)
- Quantum RAM (QRAM) is required for loading classical data - an expensive hardware component with no demonstrated large-scale implementation
- The speedup applies to the number of queries to the data structure, not necessarily wall-clock time
- **Dequantization caveat (Tang 2018):** classical algorithms using sampling access to data can in some settings match QPCA's query complexity, eliminating the exponential advantage. The regime where genuine quantum speedup survives remains narrow and contested.

QPCA is most naturally suited to **natively quantum data** (e.g., quantum states from a physical system), where the density [[Matrix]] is prepared directly without classical loading overhead.

**QPCA vs. Classical PCA**

| Feature | Classical PCA | QPCA                                                          |
| --------------- | --------------------------------------- | ----------------------------------------------------------- |
| Time complexity | $O(d^3)$ for $d$-dimensional data | $O(\log d)$ queries - under QRAM + density [[Matrix]] assumptions |
| Data handling | General classical data | Quantum-state data or QRAM-loaded classical data              |
| Output | Classical eigenvectors | Quantum states of principal components                        |

> **Note:** The $O(\log d)$ figure is a query complexity bound, not a wall-clock speedup. Classical preprocessing costs (state preparation, QRAM) can dominate and are not included in this count.

**Candidate application areas** (speculative - no demonstrated quantum advantage on classical data at scale):

- **Quantum chemistry**: Extracting dominant orbitals from quantum-state representations of molecules. Small-scale experiments have verified the circuit mechanics (e.g., 4-qubit systems), but do not demonstrate speedup over classical methods at chemically relevant scales.
- **High-dimensional data analysis**: Finance, bioinformatics - theoretically appealing if QRAM becomes practical and data is genuinely high-rank.
- **Hybrid workflows**: Combining QPCA with classical filtering (e.g., isolation forests) as a preprocessing step in hybrid pipelines.

| Usage | Challenge | Status |
| ----------------- | --------------------------------------- | ---------------------------------- |
| Financial Fraud | NISQ hardware noise; QRAM required | Research-stage only |
| Drug Discovery | Limited to low-rank molecular systems | Hybrid classical-quantum workflows |
| Image Recognition | State preparation overhead dominates | No demonstrated advantage |

**Further reading:** Lloyd, Mohseni & Rebentrost, "Quantum principal component analysis," Nature Physics 10 (2014); Tang, "A quantum-inspired classical algorithm for recommendation systems," STOC 2019.

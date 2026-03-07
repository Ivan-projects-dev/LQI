#ML #Quantum 
Hallmark of classical feedforward networks is the **nonlinear activation** in each neuron. In quantum circuits, gates are linear, so nonlinearity arises chiefly from **measurement** or other non-unitary steps:
- **Measurement-Based Activations**: qubit (or set of [[Qubits]]) is measured, producing a classical result that can be re-encoded into another qubit for the next layer (hybrid approach). This partial measurement effectively inserts a nonlinearity.
- **Non-Linear Post-Processing**: Even if no intermediate measurement is performed, the final measurement outcome & subsequent classical post-processing can yield nonlinear mappings from input to output.
- **Coherent "Activations"**: Some proposals attempt to implement approx activation func using controlled gates & ancillas, but these are often $>$ hardware-intensive & < commonly used on near-term devices.

Similar to classical feedforward networks, QFNNs can handle:
- **Binary Classification**: Map $x$ to a single qubit measurement. Threshold the expectation value $⟨Z⟩$ or interpret measurement outcomes as labels ${0,1}$ or ${−1,+1}$.
- **Multi-Class**: Use multiple Qubits or measurement operators to distinguish among $>$ classes.
- **Regression**: Obtain a continuous output by measuring an expectation value (e.g., $⟨Z⊗Z⟩$ on multiple [[Qubits]]), or convert a bitstring readout into a real-valued quantity.

Because each [[QFNN]] layer is a quantum circuit, the entire model's params $θ$ can be trained using classical optimization methods, guided by:
- **Parameter-Shift Rule**: Compute gradients by shifting gate params $θ$ by $±π2$.
- **Automatic Differentiation**: For circuit simulators that integrate well with ML frameworks
- **Gradient-Free Methods**: If gradient calcs are difficult (e.g., large circuits or hardware constraints), evolutionary strategies or black-box optimizers can be used.

**Classical-quantum feedback loop** is typically formed, wherein each update:
1. Sets params $θ$.
2. Runs the quantum circuit to compute the cost/loss.
3. Returns a measurement (classical data) to a classical optimizer.
4. Updates params & repeats.
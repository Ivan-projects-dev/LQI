#ML #Quantum 
Classical generative models, like **Restricted Boltzmann Machines** or certain neural architectures, have long been used for tasks such as creating synthetic data & anonymization. QCBMs can be viewed as the **quantum analog** of RBMs, where each qubit effectively corresponds to a visible binary unit. They offer:
1. **Potentially Higher Expressive Power**: Research suggests that with only polynomially many params, QCBMs can represent distributions that classical RBMs cannot easily capture.
2. **Fast Sampling**: Once params are set, generating a sample requires a single exec (one circuit run plus measurement), compared to the iterative Gibbs sampling required in some classical models.
3. **Quantum Data Loading**: QCBMs can directly embed quantum states, potentially enabling further quantum processing steps.

QCBM starts with all [[Qubits]] in the $|0⟩$ state & processes them through a sequence of **one-qubit gates** (parametric rotations) and **fixed $2$-qubit gates**. After the final layer, a measurement in the computational basis yields a bitstring sample. The distribution of these bitstrings is the model's learned [[Probability distribution]].
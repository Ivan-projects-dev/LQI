#Math #Q-Sharp
**SWAP gate** exchanges the quantum states of $2$ [[Qubits]]: $|ab\rangle \mapsto |ba\rangle$. It is fundamental in quantum circuit routing - used whenever gate needs to act on physically non-adjacent [[Qubits]].
$$SWAP = \begin{pmatrix}1&0&0&0\\0&0&1&0\\0&1&0&0\\0&0&0&1\end{pmatrix}$$
$$SWAP|00\rangle = |00\rangle \quad SWAP|01\rangle = |10\rangle \quad SWAP|10\rangle = |01\rangle \quad SWAP|11\rangle = |11\rangle$$
More generally: $SWAP|\psi\rangle|\phi\rangle = |\phi\rangle|\psi\rangle$ for any single-qubit states $|\psi\rangle$, $|\phi\rangle$. 
$SWAP^2 = I$ - self-inverse.
$$SWAP = CNOT_{01} \cdot CNOT_{10} \cdot CNOT_{01}$$
### SWAP in circuit routing
On hardware with limited qubit connectivity (e.g., IBM heavy-hex topology), $2$-qubit gates can only be applied between physically connected [[Qubits]]. When a circuit requires gate between non-adjacent [[Qubits]], the transpiler inserts SWAP gates to move qubit states along the connectivity graph.

Each inserted SWAP costs 3 CNOTs. A single SWAP insertion can turn a 1-[[CNOT]] circuit into 4-[[CNOT]] circuit on hardware - this is why routing overhead can dramatically increase circuit depth. See [[Circuits growth]].

**IBM heavy-hex**: each qubit connects to $2–3$ neighbors, so [[Qubits]] at distance $d$ require $d-1$ SWAPs to communicate - up to $d-1 = 4$ on $127$-qubit device.

**[[SWAP test]]**: measuring qubit overlap $|\langle\psi|\phi\rangle|^2$ uses CSWAP:
```csharp
H(ancilla);
Controlled SWAP([ancilla], (q0, q1));
H(ancilla);
// P(ancilla = 0) = (1 + |⟨ψ|φ⟩|²) / 2
```
This is used in quantum ML (comparing quantum feature vectors) & quantum fingerprinting.

SWAP is **max entangling** for appropriate inputs. Starting from product state:
$$SWAP \cdot (H \otimes I)|00\rangle = SWAP \cdot |{+}0\rangle = \frac{|00\rangle + |01\rangle}{\sqrt{2}}$$
However, SWAP on Bell state simply permutes which qubit holds which share of the entanglement - it does not destroy entanglement.
```csharp
SWAP(q0, q1); // standard SWAP
Controlled SWAP([ctrl], (q0, q1)); // CSWAP (Fredkin)

// Manual decomposition:
CNOT(q0, q1);
CNOT(q1, q0);
CNOT(q0, q1);
```
**Qiskit**
```python
qc.swap(0, 1) # SWAP
qc.cswap(0, 1, 2) # CSWAP: control=0, targets=1,2
```
**[[PennyLane]]**
```python
qml.SWAP(wires=[0, 1])
qml.CSWAP(wires=[0, 1, 2])
```

| Property               | Value                                   |
| ---------------------- | --------------------------------------- |
| [[Qubits]]             | $2$                                     |
| [[Matrix]] size        | $4 \times 4$                            |
| Self-inverse           | Yes ($SWAP^2 = I$)                      |
| Clifford gate          | Yes                                     |
| [[T gate]] cost            | $0$                                     |
| [[CNOT]] decomposition | $3$ CNOTs                               |
| Hardware role          | Routing between non-adjacent [[Qubits]] |
| Q# name                | `SWAP`                                  |
| Qiskit name            | `swap`                                  |
| [[PennyLane]] name     | `qml.SWAP`                              |

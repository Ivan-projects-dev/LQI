#Quantum #Q-Sharp 
**Quantum counting** - [[QPE]] applied to [[Grover]] operator $G$

Estimates $M$ (solution count) by finding eigenphase $\theta/\pi$ of $G$:
```csharp
operation EstimateSolutionCount(nQubits : Int, nControlBits  : Int,
    markingOracle : (Qubit[], Qubit) => Unit is Adj + Ctl) : Int {
    // Grover operator G = Diffusion ∘ PhaseOracle
    // Define as operation to pass into QPE
    let groverOp = (power : Int, register : Qubit[]) => {
        for _ in 1..power {
            ApplyPhaseOracle(markingOracle, register);
            ApplyGroverDiffusion(register);
        }
    };

    use eigenstate = Qubit[nQubits];
    ApplyToEach(H, eigenstate); // |s⟩ = equal superposition (not exact eigenstate, but works)

    let phase = EstimatePhase(nControlBits, eigenstate, groverOp);
    ResetAll(eigenstate);

    // phase ≈ θ/π or 1 - θ/π; recover M = N·sin²(θ)
    let theta = phase * PI();
    let n_f = IntAsDouble(1 <<< nQubits);
    return Round(n_f * Sin(theta)^2);
}
```

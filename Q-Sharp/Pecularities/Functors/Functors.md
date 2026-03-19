#Q-Sharp #Quantum
**Functors** in Q# are modifiers applied to operations to produce new operations with related behavior. Q# has two built-in functors: [[Adjoint op]] & [[Controlled op]].

All intrinsic gates (`H`, `X`, `Y`, `Z`, `T`, `S`, [[CNOT]], `CCNOT`, `Rx`, `Ry`, `Rz`, `R1`) support both `Adj + Ctl`. Custom operations must explicitly declare support.
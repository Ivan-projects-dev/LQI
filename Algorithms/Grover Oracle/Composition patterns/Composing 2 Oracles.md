#Algorithm #Q-Sharp 
### 1. Composing $2$ Oracles (AND of conditions)
Flip target iff **both** condition $A$ & condition $B$ hold. Allocate one [[Ancilla]] per condition, mark each separately, combine with [[Toffoli]]:
```csharp
operation MarkAAndB(register : Qubit[], target : Qubit,
    oracleA : (Qubit[], Qubit) => Unit is Adj,
    oracleB : (Qubit[], Qubit) => Unit is Adj) : Unit is Adj {
    use (ancA, ancB) = (Qubit(), Qubit());
    within {
        oracleA(register, ancA); // ancA = 1 iff condition A
        oracleB(register, ancB); // ancB = 1 iff condition B
    } 
    apply {
        CCNOT(ancA, ancB, target); // target = 1 iff both
    }
}
```
### 2. Composing $2$ Oracles (OR of conditions)
Flip target iff **either** condition $A$ or condition $B$ holds:
```csharp
operation MarkAOrB(register : Qubit[], target : Qubit,
    oracleA : (Qubit[], Qubit) => Unit is Adj,
    oracleB : (Qubit[], Qubit) => Unit is Adj) : Unit is Adj {
    use (ancA, ancB) = (Qubit(), Qubit());
    within {
        oracleA(register, ancA);
        oracleB(register, ancB);
    } apply { // OR(a,b) = NOT(NOR(a,b)) = NOT(AND(NOT a, NOT b))
        X(target); // pre-flip
        within { X(ancA); X(ancB); }
        apply  { CCNOT(ancA, ancB, target); } 
        // unflip iff NOR (= both 0 after flip = both 0 originally)
    }
}
```

Source: [microsoft/QuantumKatas - SolveSATWithGrover/ReferenceImplementation.qs (Tasks 1.5–1.6)](https://github.com/microsoft/QuantumKatas/blob/main/SolveSATWithGrover/ReferenceImplementation.qs)
```

#Q-Sharp #Algorithm 
Flip target iff qubit $k$ is $|1\rangle$. Simplest possible [[Oracle]] - just [[CNOT]]:
```csharp
operation MarkBitIsOne(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    CNOT(register[k], target);
}
```

Flip target iff qubit $k$ is $|0\rangle$ - [[CNOT]] with pre/post $X$ on the control:
```csharp
operation MarkBitIsZero(register : Qubit[], target : Qubit, k : Int) : Unit is Adj + Ctl {
    within { 
	    X(register[k]); 
	}
    apply { 
	    CNOT(register[k], target); 
	}
}
```

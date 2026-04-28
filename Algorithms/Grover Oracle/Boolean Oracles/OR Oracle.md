#Q-Sharp #Algorithm 
Flip target iff **min $1$** bit in index list is $|1\rangle$.
OR = NOT AND NOT - apply $X$ to all selected bits, check if any is still $1$, uncompute:
```csharp
operation MarkOR(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    let selected = Mapped(i -> register[i], indices);
    within {
        ApplyToEach(X, selected); // flip: 0→1, 1→0
    } apply {
        // now target fires iff original had at least one 1
        // (De Morgan: OR(x) = NOT AND(NOT x))
        // flip target iff ALL selected are now 0 (= original all-ones after negation)
        // actually: fire iff NOT all-zeros in original
        // Simplest: mark NOT(all-zero), which = NOT(all-one after flip)
        within { 
	        ApplyToEach(X, selected); 
	    } // flip back
        apply { 
	        Controlled X(selected, target); 
	    }
    }
    // Cleaner equivalent using NOR trick:
}

// Cleaner version using NOR (NOT OR = AND of negated inputs):
operation MarkOR_Clean(register : Qubit[], target : Qubit, indices : Int[]) : Unit is Adj + Ctl {
    import Std.Arrays.*;
    let selected = Mapped(i -> register[i], indices);
    // Phase 1: mark NOR (all inputs are 0)
    within {
        ApplyToEach(X, selected); // negate inputs
    } apply {
        Controlled X(selected, target); // target |= AND(NOT inputs) = NOR
    }
    // Phase 2: flip target to get OR = NOT NOR
    X(target);
}
```

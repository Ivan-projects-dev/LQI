#Q-Sharp #Quantum 
**Q# STL** has built-in namespaces that contain funcs & operations you can use in quantum programs. For example, the `Std.Intrinsic` namespace contains commonly used operations & funcs, such as `M` to measure results and `Message` to display user messages anywhere in the program.

To call func or operation, you can specify the full namespace or use an `import` statement, which makes all the funcs & operations for that namespace available & makes your code $>$ readable. Following examples call the same operation:
```c#
Std.Intrinsic.Message("Hello quantum world!");
```

```c#
// imports all funcs & operations from the Std.Intrinsic namespace.
import Std.Intrinsic.*;
Message("Hello quantum world!");

// imports just the `Message` func from the Std.Intrinsic namespace.
import Std.Intrinsic.Message;
Message("Hello quantum world!");
```
`Superposition` program doesn't have any `import` statements or calls with full namespaces. That's because the Q# dev env automatically loads $2$ namespaces, `Std.Core` and `Std.Intrinsic`, which contain commonly used funcs & operations.

You can take advantage of the `Std.Measurement` namespace by using the `MResetZ` operation to optimize the `Superposition` program. `MResetZ` combines the measurement & [[Set operations|Set operations]] into $1$ step, as in the following example:
```C#
// Import the namespace for the MResetZ operation.
import Std.Measurement.*;

@EntryPoint()
operation MeasureOneQubit() : Result {
    // Allocate a qubit. By default, it's in the 0 state.      
    use q = Qubit();  
    // Apply the Hadamard operation, H, to the state.
    // It now has a 50% chance of being measured as 0 or 1. 
    H(q);   
    // Measure & reset the qubit, & then return the result value.
    return MResetZ(q);
}
```
## Sources
- [Q# standard libraries](https://learn.microsoft.com/en-us/azure/quantum/user-guide/libraries/)
- [Std.Intrinsic API reference](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.intrinsic)
- [Q# API reference (all namespaces)](https://learn.microsoft.com/en-us/qsharp/api/)

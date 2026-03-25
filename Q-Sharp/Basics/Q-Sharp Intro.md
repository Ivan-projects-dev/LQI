#Q-Sharp #Quantum #Algorithm 
**Q#** is a high-level, open-source programming lang dev by Microsoft for writing quantum programs. Included in **Microsoft Quantum Dev Kit (QDK)**.

As quantum programming lang, Q# meets the following requirements for lang, Compiler, & runtime:
- **Hardware agnostic:** [[Qubits]] in quantum algorithms aren't tied to a specific quantum hardware or layout. Q# Compiler & runtime handle the mapping from program [[Qubits]] to physical [[Qubits]], allowing the same code to run on different quantum processors.
- **Integration of quantum & classical computing:** Q# allows for the integration of quantum & classical computations, which is essential for universal quantum computing.
- **[[Qubit management]]:** Q# provides built-in operations & funcs for managing [[Qubits]], including creating superposition states, entangling [[Qubits]], & performing quantum measurements.
- **Respect the laws of physics:** Q# & quantum algorithms must follow the rules of quantum physics. For example, you can't directly copy or access the qubit state in Q#.
Consider the following Q# program, named `Superposition`, that creates a superposition state:
```c#
namespace Superposition {
    @EntryPoint()
    operation MeasureOneQubit() : Result {
        // Allocate a qubit. By default, it's in the 0 state.  
        use q = Qubit();  
        // Apply the Hadamard operation, H, to the state.
        // It now has a 50% chance of being measured as 0 or 1.
        H(q);      
        // Measure the qubit in the Z-basis.
        let result = M(q);
        // Reset the qubit before releasing it.
        Reset(q);
        // Return the result of the measurement.
        return result;
    }
}
```
Based on the comments (`//`), the Q# program first allocates a qubit, applies operation to put the qubit in superposition, measures the qubit state, resets the qubit, & returns the result.

Q# programs can optionally start with a user-defined namespace, such as:
```c#
namespace Superposition {
    // Your code goes here.
}
```
Namespaces can help you organize related functionality. Namespaces are optional in Q# programs, meaning that you can write a program without defining namespace:
```c#
@EntryPoint()
operation MeasureOneQubit() : Result {
    // Allocate a qubit. By default, it's in the 0 state.  
    use q = Qubit();  
    // Apply the Hadamard operation, H, to the state.
    // It now has a 50% chance of being measured as 0 or 1.
    H(q);      
    // Measure the qubit in the Z-basis.
    let result = M(q);
    // Reset the qubit before releasing it.
    Reset(q);
    // Return the result of the measurement.
    return result;
}
```
Each Q# program can have only one `namespace`. If you don't specify a namespace, the Q# Compiler uses the filename as the namespace.

Every Q# program must have entry point, which is the starting point of the program. By default, the Q# Compiler starts executing a program from the `Main()` operation, if available, which can be located anywhere in the program. Optionally, you can use the `@EntryPoint()` attribute to specify any operation in the program as the point of execution.

For example, in the `Superposition` program, the `MeasureOneQubit()` operation is the entry point of the program because it has the `@EntryPoint()` attribute before the operation definition:
```c#
@EntryPoint()
operation MeasureOneQubit() : Result {
    ...
}
```
However, the program could also be written without the `@EntryPoint()` attribute by renaming the `MeasureOneQubit()` operation to `Main()`, such as:
```c#
// The Q# compiler automatically detects the Main() operation as the entry point. 

operation Main() : Result {
    // Allocate a qubit. By default, it's in the 0 state.  
    use q = Qubit();  
    // Apply the Hadamard operation, H, to the state.
    // It now has a 50% chance of being measured as 0 or 1.
    H(q);      
    // Measure the qubit in the Z-basis.
    let result = M(q);
    // Reset the qubit before releasing it.
    Reset(q);
    // Return the result of the measurement.
    return result;
}
```
Q# provides built-in DT common to most langs, including `Int`, `Double`, `Bool`, and `String`, & pes that define ranges, arrays, & tuples.

`Result` DT represents result of a qubit measurement & can have $2$ values: `Zero` or `One`.

In the `Superposition` program, the `MeasureOneQubit()` operation returns a `Result` type, which corresponds to the return type of the `M` operation. Measurement result is stored in a new variable that's defined using the `let` statement:

```c#
// The operation definition returns a Result type.
operation MeasureOneQubit() : Result {
    ...
    // Measure the qubit in the Z-basis, returning a Result type.
    let result = M(q);
    ...
}
```
Another example of a quantum-specific type is the `Qubit` type, which represents a quantum bit. Q# also allows to define own custom types. By default, Q# programs in Jupyter Notebooks use the `ipykernel` Python package. To add Q# code to a notebook cell, use the `%%qsharp` command, which is enabled with the `qsharp` Python package, followed by your Q# code.

When using `%%qsharp`, keep the following in mind:
- You must first run `from qdk import qsharp` to enable `%%qsharp`.
- `%%qsharp` scopes to the notebook cell in which it appears & changes the cell type from Python to Q#.
- You can't put a Python statement before or after `%%qsharp`.
- Q# code that follows `%%qsharp` must adhere to Q# syntax. For example, use `//` instead of `#` to denote comments and `;` to end code lines.
, see Type declarations.

In Q#, you allocate [[Qubits]] using the `use` keyword & the `Qubit` type. [[Qubits]] are always allocated in the  state. For example, the `Superposition` program defines a single qubit & stores it in the variable `q`:
```c#
// Allocate a qubit.
use q = Qubit();
```
You can also allocate multiple [[Qubits]] & access each $1$ through its index:
```c#
use qubits = Qubit[2]; // Allocate two qubits.
H(qubits[0]); // Apply H to the first qubit.
X(qubits[1]); // Apply X to the second qubit.
```
After allocating a qubit, you can pass it to operations & funcs. Operations are the basic building blocks of a Q# program. A Q# operation is a quantum subroutine, or a callable routine that contains quantum operations that change the state of the qubit register.

To define a Q# operation, you specify name for the operation, its inputs, & its output. In the `Superposition` program, the `MeasureOneQubit()` operation takes no parameters & returns a `Result` type:
```c#
operation MeasureOneQubit() : Result {
    ...
}
```
Basic example that takes no parameters & expects no return value. `Unit` value is equivalent to `NULL` in other langs:
```c#
operation SayHelloQ() : Unit {
    Message("Hello quantum world!");
}
```
Q# STL also provides operations that you can use in quantum programs, such as the Hadamard operation, `H`, in the `Superposition` program. Given a qubit in the Z-basis, `H` puts the qubit into even superposition, where it has a 50% chance of being measured as `Zero` or `One`.

While there are many types of quantum measurements, Q# focuses on projective measurements on single [[Qubits]], also known as Pauli measurements.

In Q#, the `Measure` operation measures $1>$ [[Qubits]] in the specified Pauli basis, which can be `PauliX`, `PauliY`, or `PauliZ`. `Measure` returns a `Result` type of either `Zero` or `One`.

To implement a measurement in the computational basis , you can also use the `M` operation, which measures a qubit in the Pauli Z-basis. This makes `M` equivalent to `Measure([PauliZ], [qubit])`. For example, the `Superposition` program uses the `M` operation:
```c#
// Measure the qubit in the Z-basis.
let result = M(q);
```
In Q#, [[Qubits]] **must** be in the  state when they're released to avoid errors in the quantum hardware. You can reset a qubit to the  state using the `Reset` operation at the end of the program. Failure to reset a qubit results in a runtime error.
```c#
// Reset a qubit.
Reset(q);
```

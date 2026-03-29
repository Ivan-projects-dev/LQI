#Q-Sharp
Q# organizes its STL into namespaces under `Std.*`. $2$ namespaces are auto-imported in every program: `Std.Core` & `Std.Intrinsic`. All others require explicit `import` statement.

All primitive quantum gates & measurements. Includes: `H`, `X`, `Y`, `Z`, `S`, `T`, [[CNOT]], `CCNOT`, `SWAP`, `CZ`, `Rx`, `Ry`, `Rz`, `R1`, `R`, `Exp`, `M`, `Measure`, `Reset`, `ResetAll`, `Message`.
```csharp
import Std.Measurement.*;
```
Key operations: `MResetZ`, `MResetX`, `MResetY`, `MeasureEachZ`, `MResetEachZ`, `MeasureInteger`. Used at the end of every algorithm to read out & clean up qubit registers.

```csharp
import Std.Math.*;
```
Constants: `PI()`, `E()`. funcs: `Sqrt`, `Log`, `Log2`, `Sin`, `Cos`, `Tan`, `ArcSin`, `ArcCos`, `ArcTan`, `ArcTan2`, `Floor`, `Ceiling`, `Round`, `Truncate`, `AbsD`, `AbsI`, `MaxI`, `MinI`, `MaxD`, `MinD`, `ModulusI`, `ModulusL`, `ExpModI`, `ExpModL`, `GreatestCommonDivisorI`. See [[Q-Sharp math]].

```csharp
import Std.Arrays.*;
```
Key funcs: `Length`, `Head`, `Tail`, `Most`, `Rest`, `Reversed`, `Sorted`, `Mapped`, `Filtered`, `Fold`, `Enumerated`, `Zipped`, `Padded`, `Flattened`, `IndexOf`, `Count`, `Any`, `All`, `DrawMany`. See Q# arrays & functional ops.

```csharp
import Std.Convert.*; 
```
Key: `IntAsDouble`, `IntAsBigInt`, `IntAsBoolArray`, `BoolArrayAsInt`, `ResultAsInt`, `ResultArrayAsIntBE`, `ResultArrayAsIntLE`, `BigIntAsInt`, `DoubleAsInt`. Used constantly to bridge int ↔ bit-array ↔ Result[] conversions.

```csharp
import Std.Canon.*; //high-level quantum combinators
```
Key operations: `ApplyToEach`, `ApplyToEachA`, `ApplyToEachCA`, `ApplyControlledOnInt`, `ApplyControlledOnBitString`, `ApplyIfOne`, `ApplyIfZero`, `ApplyIfOneA`, `ApplyIfZeroA`, `CX`, `CY`, `CZ`. Provides the building blocks used in [[Oracle]] construction & quantum circuit composition.

```csharp
import Std.Arithmetic.*; // quantum arithmetic
```

Key: `ApplyQFT`, `ApplyReversedOpCA`, `ApplyReversedOpCEA`. Contains the QFT implementation used in [[QPE]] in Q-Sharp. Also includes quantum adders, comparators, & modular arithmetic for Shor's algorithm implementations.

```csharp
import Std.Diagnostics.*; //simulation & debugging
```
Key: `DumpMachine`, `DumpRegister`, `CheckZero`, `Fact`, `CheckOperationsAreEqual`. Only valid in simulation - ignored or error on hardware. See Q# diagnostics & simulation.

```csharp
import Std.Random.*;
```
Key: `DrawRandomInt(min, max)`, `DrawRandomDouble(min, max)`, `DrawRandomBool(prob)`. Used in [[Grover in Q-Sharp]] exponential search to pick a random iteration count. Note: these are **classical** RNG calls, not quantum - for quantum randomness use measurement of $|+\rangle$.

```csharp
import Std.ResourceEstimation.*; 
```
Used with the [[QRE]] to annotate circuit cost. Key: `AccountForEstimates`, `BeginEstimateCaching`, `EndEstimateCaching`.
## Sources
- [Namespaces in Q#](https://learn.microsoft.com/en-us/azure/quantum/user-guide/language/programstructure/namespaces)
- [Q# API reference (all namespaces)](https://learn.microsoft.com/en-us/qsharp/api/)
- [Std.Intrinsic API reference](https://learn.microsoft.com/en-us/qsharp/api/qsharp-lang/microsoft.quantum.intrinsic)

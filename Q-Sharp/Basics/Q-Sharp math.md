#Q-Sharp #Math 
`Std.Math` provides classical math funcs & constants used throughout quantum algorithms - rotation angles, iteration counts, phase calcs, modular arithmetic (`PI()`, `E()`).

Both are funcs returning `Double`, not literal constants. Used in every rotation gate: `Rx(PI() / 2.0, q)`.

**Floating-point funcs**
```csharp
Sqrt(2.0) // √2 ≈ 1.41421
Log(x) // natural log ln(x)
Log2(x) // log base 2
Lg(x) // alias for Log2
Sin(theta) // sine (radians)
Cos(theta) // cosine
Tan(theta) // tangent
ArcSin(x) // inverse sine, returns Double in [-π/2, π/2]
ArcCos(x) // inverse cosine
ArcTan(x) // inverse tangent
ArcTan2(y, x) // atan2(y, x) - full quadrant arc tangent
Ceiling(x) // min Int ≥ x, returns Double
Floor(x) // max Int ≤ x, returns Double
Round(x) // nearest int, returns Double
Truncate(x) // truncate toward 0, returns Double (cast to Int with int cast)
AbsD(x) // |x| for Double
PowD(base, exp) // base^exp for Double
```

**Casting float → int**:
```csharp 
let n = Floor(PI() / 4.0 * Sqrt(1024.0)); // Double
let k = Truncate(n); // still Double
// use explicit cast for Int:
let kInt = Floor(n) == n ? int(n) | 0; // idiomatic: use Round or explicit
```
In practice, use `Round`, `Floor`, or `Ceiling` & en pass to `IntAsDouble` in reverse if needed.

**int funcs**
```csharp
AbsI(n) // |n| for Int
MaxI(a, b) // max of two Int
MinI(a, b) // min of two Int
MaxD(a, b) // max of two Double
MinD(a, b) // min of two Double
ModulusI(n, m) // n mod m, always non-negative (unlike n % m in Q#)
ModulusL(n, m) // same for BigInt
```
`n % m` in Q# can return negative for negative `n` - use `ModulusI` in quantum algorithms where indices must wrap correctly.

**Modular exponentiation - `ExpModI` / `ExpModL`**
$$\text{ExpModI}(b, e, m) = b^e \bmod m$$
```csharp
ExpModI(2, 10, 1000)   // 2^10 mod 1000 = 24
ExpModL(base, exp, modulus)   // BigInt version for Shor's algorithm
```

Essential in Shor's algorithm for computing the modular exponentiation unitary $U_a|x\rangle = |ax \bmod N\rangle$. `BigInt` version handles cryptographic-scale ints.

**GCD & ted**
```csharp
GreatestCommonDivisorI(a, b) // gcd(a, b) for Int
GreatestCommonDivisorL(a, b) // gcd(a, b) for BigInt
```
Used in Shor's algorithm post-processing: once order $r$ is found, compute $\gcd(a^{r/2} \pm 1, N)$ to get factors.

**`ContinuedFractionConvergentI` - rational approximation**

Converts decimal approximation to fraction $p/q$ using continued fractions:
```csharp
let (p, q) = ContinuedFractionConvergentI((numerator, denominator), maxDenominator);
```
Used in Shor's algorithm post-processing after [[QPE]] returns approximation to $s/r$ - continued fractions recovers the exact denominator $r$.

**Common patterns in quantum algorithms**

[[Grover]] iteration count:
```csharp
let k = Floor(PI() / 4.0 * Sqrt(IntAsDouble(1 <<< n) / IntAsDouble(m)));
```

QFT phase angle for $k$-th controlled rotation:
```csharp
let angle = 2.0 * PI() / IntAsDouble(1 <<< k);
```

[[QPE]] phase decoding:
```csharp
let phi = IntAsDouble(measured) / IntAsDouble(1 <<< t); // φ ≈ measured / 2^t
let M = Round(IntAsDouble(1 <<< n) * Sin(phi * PI()) ^ 2.0); // quantum counting
```

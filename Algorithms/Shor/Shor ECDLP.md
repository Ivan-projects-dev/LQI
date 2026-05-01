#Algorithm #Math #Q-Sharp 
**Elliptic Curve Discrete [[Logarithm]] (ECDLP)**

**Target:** Find $k$ given $P$ & $Q = kP$ on elliptic curve over $\mathbb{F}_p$
**Group:** elliptic curve group $E(\mathbb{F}_p)$ under point addition (abelian group)

**Algorithm:** Same $2D$ [[QPE]] structure as [[Shor]] DLP, with the [[Oracle]] computing **elliptic curve point multiplication** $Q \cdot a$ & $P \cdot (-b)$ in superposition.

[[Oracle]] replaces modular multiplication with point addition on the curve. Circuit for controlled point addition is $>$ complex than controlled modular multiplication - it requires arithmetic in $\mathbb{F}_p$ (field arithmetic: modular add, multiply, invert) to implement the group law.

**Resource comparison at $128$-bit classical security:**

| Algorithm | Classical security (bits) | Quantum logical [[Qubits]] | Physical [[Qubits]] |
|---|---|---|---|
| [[RSA]]-3072 | 128 | $\sim 9{,}000$ | $\sim 9{,}000{,}000$ |
| [[ECDH]] P-256 | 128 | $\sim 2{,}330$ | $\sim 2{,}000{,}000$ |

[[ECDH]] uses smaller keys but is broken by a **smaller** quantum computer than [[RSA]] at the same classical security level. This is a key reason post-quantum migration covers both [[RSA]] & elliptic curves.

**Circuit for elliptic curve point addition** uses:
- Modular field arithmetic ($\mathbb{F}_p$): about $3$ multiplications & $1$ inversion per point addition
- $1$ point addition ≈ $10n$ [[Toffoli]] gates (for $n$-bit $p$)
- Full scalar multiplication ($k$ times point addition): $O(n^2)$ [[Toffoli]] gates

Same $2$-register [[QPE]] structure as [[Shor]] DLP applies. [[Oracle]] is:
```Csharp
// DLP oracle for elliptic curve group
// aReg encodes exponent a, bReg encodes exponent b
// output initialized to O (point at infinity = identity)
operation ECDLPOracle(aReg : Qubit[], bReg : Qubit[],
    output : Qubit[], // encodes an EC point (x,y) in Montgomery form
    P : ECPoint, // generator (classical)
    Q : ECPoint, // target point Q = kP (classical)
    curve : ECParams // p, a, b for y^2 = x^3 + ax + b (classical)
) : Unit is Adj + Ctl {
    // Controlled scalar multiplication: output += a * P (EC point addition)
    let n = Length(aReg);
    mutable Pk = P;
    for i in 0..n-1 {
        ControlledPointAdd(aReg[i], output, Pk, curve);
        set Pk = PointDouble(Pk, curve);   // P, 2P, 4P, 8P, ...
    }
    // Controlled subtraction: output -= b * Q  (= output += b * (-Q))
    let negQ = ECPointNegate(Q, curve);
    mutable Qk = negQ;
    for i in 0..n-1 {
        ControlledPointAdd(bReg[i], output, Qk, curve);
        set Qk = PointDouble(Qk, curve);
    }
}
```
**Breaks:** [[ECDH]], ECDSA, all elliptic curve protocols including TLS with P-$256$/P-$384$, secp256k1 (Bitcoin), $Ed25519$.

**Reference:** Roetteler, Naehrig, Svore, Lauter (2017) - $1st$ concrete quantum resource estimate for ECDLP.
#Math 
**Classical autoencoder (CAE)** is a [[Neural network]] trained to replicate its own input at the output layer. It typically has:
1. **Input layer** of dimension $n$.
2. $1>$ **hidden layers**, where at least $1$ hidden layer (the "bottleneck") has fewer neurons than the input.
3. **Output layer**, also of dimension $n$.
Mathematically, if $x∈R_n$ is the input, the autoencoder decomposes into:
$z=f_(enc)(x;w_(enc)),^x=f_(dec)(z;w_(dec))$
where:
- $f_(enc)$ is the **encoder**, mapping $x$ to a lower-dimensional [[Vector]] $z$.
- $f_(dec)$ is the **decoder**, reconstructing $x∈R_n$.
Training minimizes a loss: $L(w)=∑i∥∥x_i−'x_i∥∥2$ where $w={w_(enc), w_(dec)}$. Once trained, the encoder effectively **compresses** data to $z$, while the decoder **decompresses** it.
![[Pasted image 20260101164517.png]]

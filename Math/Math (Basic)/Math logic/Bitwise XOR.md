#Math 
**Bitwise XOR** operator $⊕$ (also known as bitwise addition modulo $2$) takes $2$ bit strings as inputs & performs an exclusive OR operation on every bit pair from the $2$ bit strings. XOR operator returns $1$ if the bit pair is different & $0$ if the bit pair is the same.

One can say that the operator outputs a true value when either of the input bits are exclusively true very much similar to an bitwise addition modulus $2$ (or a bitwise addition without the carry bit).

Applying the bitwise XOR operator to a random bit string $r : (r ∈ {0,1}n)$ & the $0$ bit string $z (z=0n)$ will return $r$ as the output. Recall the bitwise XOR table for $2$ bits $ABA⊕B000011101110$

Let $a_1=101, a_2=011, a_3=100, a_4=000$.
$a_1⊕a_2=(1,0,1)⊕(0,1,1)=(1⊕0,0⊕1,1⊕1)=110$
$a_1⊕a_3=(1,0,1)⊕(1,0,0)=(1⊕1,0⊕0,1⊕0)=001$
$a_2⊕a_3=(0,1,1)⊕(1,0,0)=(0⊕1,1⊕0,1⊕0)=111$
$a_1⊕a_4=(1,0,1)⊕(0,0,0)=(1⊕0,0⊕0,1⊕0)=101$

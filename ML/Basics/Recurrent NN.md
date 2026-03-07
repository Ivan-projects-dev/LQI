#ML 
**Recurrent [[Neural network]] (RNN)** processes sequential data by maintaining a hidden state $ht$. At each time step $t$, with input $xt$,  $ht=f(Whhht−1+Wxhxt+bh),yt=Whyht+by$ where $f(⋅)$ is a (nonlinear) activation like tanh, & $yt$ is the output at step $t$. RNNs learn temporal or sequential patterns in time series, lang, etc. LSTMs and GRUs are popular variants that mitigate **vanishing/exploding gradients** via gating mechanisms.
![[Pasted image 20260101164132.png]]

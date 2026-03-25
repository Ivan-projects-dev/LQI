#ML 
**Classical feedforward [[Neural network]]** (**Multilayer [[Perceptron]]/MLP**) consists of an **input layer** that receives data x, $1$ or $>$ **hidden layers** that successively transform the data through linear operations & nonlinear activations, & an **output layer** that produces the network's final prediction (e.g., a class label or regression value). 

Each hidden layer typically computes $z(ℓ)=σ(W(ℓ)z(ℓ−1)+b(ℓ))$ where
	$W(ℓ)$ and $b(ℓ)$ are learnable weights & biases, 
	$σ$ is nonlinear activation func
	$z(ℓ)$ represents the **activation [[Vector]]** produced by the $ℓ$ layer. It is the **output** of $ℓ$ layer, which becomes the **input** of the next layer $ℓ+1$. Each component of $z(ℓ)$ would correspond to the activation value of a "neuron" or unit of that layer.
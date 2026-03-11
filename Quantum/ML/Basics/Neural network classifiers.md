#ML #Math
![[Pasted image 20250715204843.png]]
[[Neural network]] has multiple layers, each acts as input to next later.
First layer has input nodes, which are assigned values from input attrs.
Each node combines values of its inputs using some weight func to compute its value 
	• Weights are associated with edges 
For classification, each output value indicates likelihood of the input instance belonging to that class 
	• Pick class with max likelihood
Weights of edges are key to classification 
Edge weights are learnt during training phase

Value of a node may be linear combo of inputs, or may be a non-linear func (sigmoid func).
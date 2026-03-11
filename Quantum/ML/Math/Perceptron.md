#ML 
**Perceptron** is artificial neuron defines the first step into [[Neural network]] (simplest possible [[Neural network]] itself). It represents a single neuron with only one input layer, & no hidden layers.
![[Pasted image 20251007200133.png]]
Original **Perceptron** was designed to take a num of **binary** inputs, & produce one **binary** output ($0$ or $1$). The idea was to use different **weights** to represent the importance of each **input**, & that the sum of the values should be greater than a **threshold** value before making a decision (like true or false).

[[Frank Rosenblatt]] suggested this algorithm:
1. Set a threshold value
2. Multiply all inputs with its weights
3. Sum all the results
4. Activate the output

During training, the perceptron adjusts its weights based on observed errors. This is typically done using a learning algorithm such as the perceptron learning rule or a backpropagation algorithm.
Learning process presents the perceptron with labeled examples, where the desired output is known. Perceptron compares its output with the desired output & adjusts its weights accordingly, aiming to minimize the error between the predicted & desired outputs.

Learning process allows the perceptron to learn the weights that enable it to make accurate predictions for new, unknown inputs, but they are limited for linearly separable patterns.
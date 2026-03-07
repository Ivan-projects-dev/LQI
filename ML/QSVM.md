#Quantum #Math #ML
**Quantum SVM** is an adaptation of SVM that utilizes quantum computing to enhance its performance. SVM is supervised learning algorithm widely used for classification tasks, while QSVM utilizes quantum kernel func to map data in Hilbert space.

To transform our classical SVM to quantum SVM, we start by changing our kernel func. As shown before, we need to calculate the product of $φ(x_i) • φ(x_j)$. We can substitute $φ$ as [[Quantum state]] as gate $U$ applied to an init state:![](https://lms.qureca.com/wp-content/uploads/uncanny-snc/25/assets/Screenshot%202025-03-13%20163201.png)
By using this quantum representation of our transformation, we can assume that our data will be transformed through rotations, & we can create new feature map what would be defined as:
![](https://lms.qureca.com/wp-content/uploads/uncanny-snc/25/assets/Screenshot%202025-03-13%20164110.png)
Then, given the elements of $x_i$ and $x_j$, we can perform the dot product of our new mapping in terms of quantum states:
![](https://lms.qureca.com/wp-content/uploads/uncanny-snc/25/assets/Screenshot%202025-03-13%20164351.png)
And because we are only dealing with the real products of this state to make sense of the minimization problem, one possible result to the above state would be:
![](https://lms.qureca.com/wp-content/uploads/uncanny-snc/25/assets/Screenshot%202025-03-13%20164541.png)
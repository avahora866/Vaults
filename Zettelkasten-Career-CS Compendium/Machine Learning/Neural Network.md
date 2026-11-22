---
tags:
  - "#SECONDARY_BRANCH"
---
# Artificial Neuron (Perceptron)
A Neural network is composed of many artificial neurons (or perceptron), often called nodes. An artificial neuron is a mathematical function that takes multiple inputs, applies a specific weight to each input, sums them up and adds a bias. It then passes the result through an activation function to determine the output

$$
z = \sum (x \cdot w) + bias
$$
- x = inputs (vector)
- w = weights (vector)
- z = resulting scalar

Given the following parameters:
- **Input Vector ($\mathbf{x}$):** $[2, -1]$
- **Weight Vector ($\mathbf{w}$):** $[3, 4]$
- **Bias ($b$):** $-1$

The resulting scalar is:
- $2 * 3 = 6$
- $-1 * 4 = -4$
- $6+ -4 = 2$
- $2 + -1 = 1$
z = 1

This value is then used as the input of the final component of the neuron, The "Activation Function". This function decides on weather the neuron "fires" (transmits a signal) and how strong that signal should be. It transforms the linear $z$ to the final output $a$ (activation).

$$a = f(z)$$
## Activation Functions
There are several types of activation functions however the most common ones are:
### Sigmoid Function ($\sigma$)
Visually it is an "S" curve and it squashes any number (from $-\infty$ to $+\infty$) into a range between 0 and 1. It is mainly used for probability i.e. if the output is 0.9, the network is 90% sure.
Formula
$$\frac{1}{1 + e^{-z}}$$
### ReLU (Rectified Linear Unit)
Visually it is represented as a ramp and it converts any number into either $z$ if $z$ is positive or 0 if $z$ is negative. This is the industry standard for hidden layers in modern deep learning because it is computationally efficient and solves deep mathematical problems regarding training speed
$$max(0, z)$$
---
Activation functions are used to introduce non-linearity as without these functions the neural network is just a giant Linear Regression Model. It could only draw straight lines through data. Activation functions allow the network to bend, twist and curve its understanding to fit complex data.

# Neural Network Architecture
Neurons are organised into layers. A standard Neural Network (often called a Multi-Layer Perceptron or MLP) has three distinct types of layers.

1. The Input Layer: This is the entry point and consists of the input variables. These are not neurons, they do not calculate anything, they are simply placeholders for input variables.
2. The Hidden Layer: This is where the computation actually happens. This layer sits between the input and output layers. They are "hidden" as we do not see their inputs or outputs directly in the training data, they are internal abstractions. Deep Learning is simply a neural network with many (deep) hidden layers
3. The Output Layer: The final decision. The number of nodes here depend on your goal:
	- Predicating House Price: 1 node (continuous number)
	- Classifying "Cat" vs "Dog": 1 node (0 or 1) or 2 nodes (probabilities for each)
	- Recognising hand-written digits (0-9): 10 nodes

## Matrix section of neural networks
As Neural Networks get bigger we would have a larger amount of neurons to calculate for. This is inefficient and is where matrix's comes in. Instead of looping through each neuron in the layer and calculating their output we instead use matrix multiplication to calculate the whole layer at once.

If we have a hidden layer with 4 neurons and an input vector x of size 3:
Instead of:
- $b = bias$
- $x = [x1, x2, x3]$
- $w = [w1, w2, w3]$

We have
- $b = bias$
- $x = [x1, x2, x3]$
- w = matrix of (4, 3)
	- Rows = number of neurons
	- Columns = number of inputs

|          | Input 1 | Input 2 | Input 3 |
| -------- | ------- | ------- | ------- |
| Neuron 1 | weight  | weight  | weight  |
| Neuron 2 | weight  | weight  | weight  |
| Neuron 3 | weight  | weight  | weight  |
| Neuron 4 | weight  | weight  | weight  |

The formula then becomes
$$\mathbf{Z} = \mathbf{W} \cdot \mathbf{x} + \mathbf{b}$$

$$\mathbf{A} = ReLU(\mathbf{Z})$$
This single line of linear algebra calculates the weighted sums for all neurons in that layer simultaneously. This is why GPUs are so important for AI - they excel at large matrix multiplications.

# Loss Function
The network takes an input, multiplies it by weights, adds biases, applies activations (Forward Propagation) and gives a prediction.

However, initially the weights are random. The network knows nothing. It will guess randomly. To teach it, we must quantify exactly how wrong it is. This is where the Loss Function (or Cost Function) comes in

The Loss Function takes the Network's Prediction ($\hat{y}$) and compares it with the Actual Truth ($y$) from our dataset.

## Common Loss Functions
### Mean Squared Error (MSE) 
Used for regression - when predicting continuos numbers (house prices)
$$L = (\hat{y} - y)^2$$
Why square it? It ensures the error is always positive (so negatives don't cancel out positives) and it penalizes large errors more severely.

### Binary Cross-Entropy
Used for classification - when predicting probability (fraud detection)
$$L = -[y \cdot \ln(\hat{y}) + (1 - y) \cdot \ln(1 - \hat{y})]$$
Intuitively, if the Truth is 1 and you predict 0.1, the Log Loss will be massive. If you predict 0.9, the loss will be tiny

# Backpropogation
Now that we know the error of the neural network through the Loss Function we need to tweak the neural network to minimise it.

To find the correct weights of the inputs we start with Random Weights (garbage (initial forward propogation)) and the network adjusts them automatically.
This process is called Backpropogation using Gradient Descent

## Gradient
Gradient in this context refers to the gradient in calculus. This is simply a vector. It points in direction of the steepest ascent. 
- If the Gradient Vector is $[2.5, -1.0]$, it means "If you increase Weight 1, Error goes WAY up. If you increase Weight 2, Error goes down."

$\alpha$ (Alpha) is the Learning Rate: The size of the step you take.

- High Gradient (Steep Slope): This weight is very important. Changing it slightly will drastically change the error.
- Low Gradient (Flat Slope): This weight doesn't matter much. Changing it won't affect the error significantly.

### Calculating the Gradient
Imagine we are looking at one specific weight ($w$) in the Output Layer.When you change this weight slightly, it triggers a chain reaction:

1. Change $w$ $\rightarrow$ Changes the Weighted Sum ($z$).\
	- (Recall $z = w \cdot x + b$)
2. Change $z$ $\rightarrow$ Changes the Activation ($a$).
	- (Recall $a = \sigma(z)$)
3. Change $a$ $\rightarrow$ Changes the Loss ($L$).
	- (Recall $L = (a - y)^2$ or similar)

To find the Gradient (the total effect of $w$ on $L$), we simply multiply the effects of these three steps together.
$$\text{Gradient} = \frac{\partial L}{\partial w} = \underbrace{\frac{\partial L}{\partial a}}_{\text{Step 3}} \cdot \underbrace{\frac{\partial a}{\partial z}}_{\text{Step 2}} \cdot \underbrace{\frac{\partial z}{\partial w}}_{\text{Step 1}}$$

1. The Error Term ($\frac{\partial L}{\partial a}$)
	- Question: If the prediction ($a$) changes, how much does the Loss ($L$) change?
	- Math: If $L = \frac{1}{2}(a - y)^2$, the derivative is simply $(a - y)$.
	- Intuition: This is just the raw Error.
		- If you predicted 0.8 and the truth was 1.0, this term is $-0.2$.
2. The Sensitivity Term ($\frac{\partial a}{\partial z}$)
	- Question: If the linear sum ($z$) changes, how much does the Activation ($a$) change?
	- Math: This is the Slope of the activation function at that specific point.
	- The Sigmoid Magic: The derivative of the Sigmoid function happens to be $a \cdot (1 - a)$.
		- If $a$ is very high (near 1) or very low (near 0), the slope is flat (near 0). The neuron is "saturated" and refuses to learn.
		- If $a$ is in the middle (0.5), the slope is steep. The neuron learns quickly.
3. The Input Term ($\frac{\partial z}{\partial w}$)
	- Question: If the weight ($w$) changes, how much does the sum ($z$) change?
	- Math: Since $z = w \cdot x + b$, if you increase $w$ by 1, $z$ increases by exactly $x$.
	- Intuition: The Input ($x$) acts as a volume knob. If the input is 0, changing the weight does nothing (Gradient = 0). If input is 10, changing the weight has a huge 10x effect.

So, the Gradient for a specific weight is:
$$\text{Gradient} = \underbrace{(a - y)}_{\text{Error}} \cdot \underbrace{\sigma'(z)}_{\text{Slope}} \cdot \underbrace{x}_{\text{Input}}$$

This formula tells us exactly when a neuron learns:
- It learns if there is an Error (Term 1).
- It learns if the neuron is active/sensitive (Term 2).
- It learns if the Input was non-zero (Term 3).
- If any of these are zero, the Gradient is zero, and the weight does not change.

## Chain Rule
Updating the weights connected to the Output Layer is easy. We know the error right there (e.g., "You predicted 3, but the answer is 1").

But what about the weights deep inside the network (in the Hidden Layers)? The Hidden Neurons don't interact with the final answer directly. They only talk to the Output layer. So, how do we know if a specific weight in the first layer is responsible for an error in the last layer?

The Flow of Information:
- Data flows Forward (Input $\to$ Hidden $\to$ Output).
- Error (Gradient) flows Backward (Output $\to$ Hidden $\to$ Input).
This is why it is called Back-propagation. We calculate the gradient for the last layer first, then pass that information back to the layer before it, and so on, until we reach the beginning.

We use the Chain Rule (from Calculus) to calculate exactly how much each specific weight contributed to the error.

---

The Update Rule: We take the current weights ($\mathbf{W}$) and subtract a tiny fraction of the Gradient.
$$\mathbf{W}_{new} = \mathbf{W}_{current} - (\alpha \cdot \text{Gradient})$$

The "Minus" Sign is Critical
- If the Gradient is Positive (Uphill): It means increasing the weight increases the error. We want to lower the error, so we must decrease the weight.
$$W - (\text{Positive}) = \text{Smaller } W$$
- If the Gradient is Negative (Downhill): It means increasing the weight decreases the error. 
$$W - (\text{Negative}) = W + \text{Positive} = \text{Larger } W$$
The math automatically handles the direction for us.

# The Grand Loop: Training
When you hear "Training a Model," it is simply a loop of these three steps, repeated millions of times:
1. Forward Pass: Data goes in $\to$ Prediction comes out.
2. Calculate Loss: Compare Prediction to Truth.
3. Backward Pass: Calculate Gradients and update Weights (using the Learning Rate).

One full cycle through your entire dataset is called an Epoch.
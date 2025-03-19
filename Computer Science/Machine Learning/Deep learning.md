---
Tags: 
Created: 2025-03-02 23:34:16
---
(Links:: [[Machine Learning]])
# Automatic Differentiation
![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0009.svg)

The forward pass of the above neural network can be written as matrix multiplications: $${\color{orange}\begin{bmatrix}w_{11} & w_{12} \\ w_{12}& w_{22} \\ w_{13}& w_{23}\end{bmatrix}} \times \begin{bmatrix}x_{1}\\ x_{2}\end{bmatrix} + {\color{#58F}\begin{bmatrix}b_{1}\\ b_{2} \\ b_{3}\end{bmatrix}} = {\color{#0C8}\begin{bmatrix}k_{1} \\ k_{2} \\ k_{3}\end{bmatrix}} \overset{\sigma}{\to} {\color{#A8F}\begin{bmatrix}h_{1} \\ h_{2} \\ h_{3}\end{bmatrix}}$$ $${\color{orange}\begin{bmatrix}v_{1} & v_{2} & v_{3}\end{bmatrix}} \times {\color{#A8F}\begin{bmatrix}h_{1} \\ h_{2} \\ h_{3}\end{bmatrix}} + {\color{#58F}c} = {\color{#F05}y}$$ $$f(x)={\color{orange}V} \sigma({\color{orange}W}x+{\color{#58F}b})+{\color{#58F}c}$$
- vectors, matrices and high dimensional objects are called **tensors** ![|500](https://mlvu.github.io/lecture07/32.DeepLearning1.key-stage-0007anim4.svg)
	- **rank**: number of dimensions
- images can be represented with 3-tensor (rgb), and image datasets with 4-tensor
- functions (ops) can have multiple *inputs* and *outputs*. All inputs, outputs are tensors
- functions implement: 
	- `forward(...)`: computing the outputs given the inputs
	- `backward(...)`: computing the gradients for the inputs given the gradients for the outputs
- **automatic differentiation**: chain together functions, keeping track of computations in computation graph

| **lazy execution**                                                       | **eager execution**                                                                                         |
| ------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| define computation graph, compile it, iterate backward/forward over data | build computation graph on the fly during the forward pass                                                  |
| pro: fast, many possibilities for optimization                           | pro: easy to debug; problems in the model occur as the module is executing; model can be different each run |
| con: difficult to debug, model must remain static during training        | con: more difficult to optimize                                                                             |
| Examples: Keras and Tensorflow 1                                         | Examples: Pytorch and Tensorflow 2                                                                          |

- nodes the computation graph are tensor objects which store *both* data and the gradient (during back-propagation)
	- result also stores references to input and computation
	- compute the partial derivatives of c with respect to every node in the graph
# Tensor back-propagation

> [!important]
> - Functions can have any number of inputs and outputs
> - Inputs and outputs can be tensors of any rank
> - The final output must be a scalar (usually the loss)

- the loss we define over the outputs (nodes) needs to map them all to a single scalar value -> computation for back-propagation always has a single scalar output

> [!question]- What if a computation takes multiple paths in the computation tree?
> - **Multivariate chain rule**: take single derivative for each input and sum them ![|500](https://mlvu.github.io/lecture07/32.DeepLearning1.key-stage-0029anim1.svg)

$$l = \sum\limits(y - t)^{2}\qquad y= Vh +c \qquad h = \sigma(k) \qquad k= Wx+b$$
![|500](https://mlvu.github.io/lecture07/32.DeepLearning1.key-stage-0032anim2.svg)

- to speed up back-propagation, we put everything in terms of matrices (especially the local derivatives)
	
> [!info]+ Derivatives of functions with tensor inputs/outputs
> $$\begin{array}{cc|ccc}
> & & & \text{Function Output} \\
> & & \text{scalar} & \text{vector} & \text{matrix} \\ \hline
> & \text{scalar} & \text{scalar} & \text{vector} & \text{matrix} \\
> \text{Input} & \text{vector} & \text{vector} & \text{matrix} & \text{?} \\
> & \text{matrix} & \text{matrix} & \text{?} & \text{?} \\
> \end{array}$$
> 
> > [!example]- [[Linear models and search#Gradient Descent|Gradient]]
> > The function has as input a vector and as output a scalar. Thus we must take the derivative of the output with respect to each input

- multiplication of tensors with 3+ rank is not easily defined and thus makes the following very hard to compute $$\frac{\partial l}{\partial {\color{orange} W}} = \frac{\partial l}{\partial {\color{#F05}y}} \frac{\partial {\color{#F05}y}}{\partial {\color{#A8F}h}} \frac{\partial {\color{#A8F}h}}{\partial {\color{#0C8}k}} \frac{\partial {\color{#0C8}k}}{\partial {\color{orange}W}}$$
	- since we assume the function always has a **scalar output** we don't have to worry
- **Notation**: 
	- $\partial l/\partial {\color{orange} W}$: Gradient of $l$ with respect to ${\color{orange} W}$ or the gradient *for* ${\color{orange} W}$
	- Commonly written as $\nabla_{\color{orange}W}l$
	- $\nabla_{\color{orange}W}l = {\color{orange}W}^{\nabla}$ (this course only)
- the derivatives of the loss (wrt some input) will have the same shape as the tensor we're taking the derivative over ($\nabla_{\color{orange}W}l$ has the *same shape* as ${\color{orange} W}$)
- **The gradient of any tensor $T$ always has the same shape as $T$**

> [!example]- Back-propagation
> For the first layer in the feedforward network $${\color{#0C8}k}={\color{orange}W}x + {\color{#58F}b}$$ the forward and backward functions look as follows:
> - $\text{forward}({\color{orange}W},x,{\color{#58F}b}) \to {\color{orange}W}x+{\color{#58F}b}$
> - $\text{backward}({\color{#0C8}k}^{\nabla}) \to {\color{orange}W}^{\nabla},x^{\nabla},{\color{#58F}b}^{\nabla}$ where $$\begin{align*} {\color{orange}W}^{\nabla} &= {\color{#0C8}k}^{\nabla}x^{\top} \\ x^{\nabla} &= {\color{orange}W}^{\top}{\color{#0C8}k}^{\nabla}\\ {\color{#58F}b}^{\nabla}&= {\color{#0C8}k}^{\nabla} \end{align*}$$

Working out derivatives for high rank tensors:
1. Describe the problem in terms of *scalar derivatives*
2. Apply the scalar (multivariate) chain rule
3. Rewrite the scalar computations as tensor operations

> [!example]-
> $${\color{#0C8}k}={\color{orange}W}x + {\color{#58F}b}$$
> $$\begin{align*}
> \frac{\partial l}{\partial {\color{orange}W_{32}}} &= \sum\limits_{\color{#0C8}i}\frac{\partial l}{\partial {\color{#0C8}k_{i}}}\frac{\partial {\color{#0C8}k_{i}}}{\partial {\color{orange}W_{32}}}\\
> &= \sum\limits_{\color{#0C8}i}\frac{\partial l}{\partial {\color{#0C8}k_{i}}}\frac{\partial ({\color{orange}W} x + {\color{#58F}b})_{\color{#0C8}i}}{\partial {\color{orange}W_{32}}}\\
> &= \sum\limits_{\color{#0C8}i}\frac{\partial l}{\partial {\color{#0C8}k_{i}}}\frac{\partial ({\color{orange}W}_{{\color{#0C8}i}\,\cdot}\, x + {\color{#58F}b}_{\color{#0C8}i})}{\partial {\color{orange}W_{32}}}\\
> &= \sum\limits_{\color{#0C8}i}\frac{\partial l}{\partial {\color{#0C8}k_{i}}}\frac{\partial \sum_{j}({\color{orange}W}_{{\color{#0C8}i}j}\, x_{j})}{\partial {\color{orange}W_{32}}}\\
> &= \sum\limits_{{\color{#0C8}i}j}\frac{\partial l}{\partial {\color{#0C8}k_{i}}}\frac{\partial {\color{orange}W}_{{\color{#0C8}i}j}\, x_{j}}{\partial {\color{orange}W_{32}}} = \frac{\partial l}{\partial {\color{#0C8}k_{3}}}\frac{\partial {\color{orange}W}_{{\color{#0C8}3}2}\, x_{2}}{\partial {\color{orange}W_{32}}} = \frac{\partial l}{\partial {\color{#0C8}k_{3}}}x_{2}
> \end{align*}$$

Calculating each element separately would take too long, so we compute using matrix operations
$${\color{orange}W_{ij}}^{\nabla}=\frac{\partial l}{\partial {\color{orange}W_{ij}}} = \frac{\partial l}{\partial {\color{#0C8}k_{i}}}x_{j}={\color{#0C8}k_{i}}^{\nabla}x_{j}$$
$${\color{orange}W}^{\nabla}=\begin{bmatrix}{\color{#0C8}k_{1}}^{\nabla}x_{1}  & {\color{#0C8}k_{1}}^{\nabla}x_{2} \\ {\color{#0C8}k_{2}}^{\nabla}x_{1} & {\color{#0C8}k_{2}}^{\nabla}x_{2} \\ {\color{#0C8}k_{3}}^{\nabla}x_{1} & {\color{#0C8}k_{3}}^{\nabla}x_{2}\end{bmatrix}={\color{#0C8}k}^{\nabla}x^{\top}$$

# Convolutions
- hidden layer connect to a neighbourhood of inputs (not all) all with the same weights
	- different channels have different weights
	- set of weights applied to each "patch" is the kernel
	- padding necessary for inputs that only contribute few hidden nodes

> [!question]- For a 6x6 image, with 1 pixel of padding, 1 input channel and 3 output channels, a 3x3 kernel, and stride 1: what is the size of the output tensor and how many (unique) weights does the convolution have?
> 1. 6x6x3
> 2. $9 \text{ (kernel) }\times 3 \text{ (channels) }=27$

- high resolution, few channels/weights; low resolution, many channels/weights
- output layer has the shape of another "image", with more channels
- output nodes are only wired to *nearby* nodes in the previous layer
- Weights are shared, each hidden node has the same weights to the previous layer
- Max-pooling reduces the image dimensions

# Making it work
- **vanishing gradient**: the derivative of the sigmoid is at most 0.25, thus the loss will always be small (and continue to shrink with more layers)
- ReLU fixes this issue, however with it a neuron can become dead when always producing a negative input
- There are two standard initialisation mechanisms (both assume normalized data): Make weights a random orthogonal matrix (eigenvalues all 1)
- **minibatch** gradient descent: 
	- Like stochastic gradient descent, but with small batches of instances, instead of single instances
	- Smaller batches: more like stochastic GD, *more noisy*, *less parallelism*
	- Bigger batches: more like regular GD, *more parallelism*, limit is memory
	- General advice, keep it between 16 and 128 instances
- optimizers: attempt to adapt GD update rule to improve convergence
	- Nesterov momentum: $$\begin{align}{\color{#58F}v} &\leftarrow \mu {\color{#58F}v} - \eta\nabla \text{loss}({\color{orange}w}) \\ {\color{orange}w} &\leftarrow {\color{orange}w} + {\color{#58F}v}\end{align}$$
	- momentum vector gives every weight a separate momentum scalar that changes how much that weight will change separate from all the other weights

---
References:
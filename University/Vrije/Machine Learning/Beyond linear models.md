---
Tags: 
Created: 2025-03-02 23:33:39
---
(Links:: [[Machine Learning]])
# Neural Networks
- **feedforward network**: feature extractor (we don't specify features, but they are learnt) followed by linear model
	- layer of **hidden units** (non-linear) connecting to all input and output nodes
	- nodes in same layer are not connected to another or any layer other than next and previous ones
	- each layer **fully connected** to previous layer
- **support vector machine**: uses *kernel function* to fit linear models in very high-dimensional feature space
- [[Percom 7 Classification II#Perceptron|Perceptron]]: a number of inputs (features) each multiplied with a weight, summed together with a *bias* parameter -> sign of result used for classification $$y=w_{1}x_{1}+w_{2}x_{2}+b$$
- **composing perceptrons doesn't make them more powerful** (the result is never more than a simple linear function)
	- make them more expressive by using **non-linearity** (**activation function**)
	- **logistic sigmoid** (output always in interval $[0,1]$) or **ReLU** (every negative input is zero; everything else is the same)

![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0009.svg)
```python
for j in 1 ... 3:
	for i in 1 ... 2:
		k_j += w_ij * x_i
	k_j += b_j

for i in 1 ... 3:
	h_i = sigmoid(k_i)

for i in 1 ... 3:
	y += h_i * v_i
y += c

loss = (y - t) ** 2
```
- use *one* output to build a **regression model**
- number of hidden nodes modelled as hyperparameter
- apply logistic sigmoid to output and interpret result as probability given input that it is positive ([[Probabilistic models#Logistic Regression|logistic regression]])
- multiclass classification: create output node per class, **they are all positive and together sum to one** -> output is **class probability**
	- softmax activation: output is exponentiated and divided by sum total $$o_{i}=w^{\top}h+b \qquad y_{i}=\frac{\text{exp}(o_{i})}{\sum_{j}\text{exp}(o_{j})}$$
- *stochastic gradient descent*: loss function over a single example instead of summing over whole dataset

> [!summary] Training a neural network
> - get some examples of input and output
> - get a loss function (least squares, cross entropy)
> - work out the gradient of the loss with respect to the weights
> - use (stochastic) gradient descent to improve the weights bit by bit
# Local and global derivatives
- **Back-propagation**:
	- Breack your computation down into a chain of *modules*
	- Work out the derivative of each module with respect to its input *symbolically*
	- *Compute* the global gradient for a given input by multiplying these gradients
	- Accumulate your gradients down the computation graph
- ${\color{#58F}\text{global derivative}}$: derivative of the whole function with respect to input $x$
- ${\color{#F05} \text{local derivative}}$: derivative of each module with respect to its input 

$$f(x)=d(c(b(a(x))))$$
$${\color{#58F}\frac{\partial f}{\partial x}}={\color{#F05} \frac{\partial d}{\partial c}} \frac{\partial c}{\partial b} \frac{\partial b}{\partial a} \frac{\partial a}{\partial x}$$

> [!example]-
> $$f(x)={\color{#F05} \frac{2}{ {\color{orange} \sin ( {\color{#A8F} e^{\color{#0C8} - {\color{#58F}x}}} )} }}$$
> $$
> \begin{array}{cc}
> \begin{align}
> {\color{#F05}d}(c) &= \frac{2}{{\color{orange}c}} \\
> {\color{orange}c}(b) &= \sin {\color{#A8F}b} \\
> {\color{#A8F}b}(a) &= e^{{\color{#0C8}a}} \\
> {\color{#0C8}a}(x) &= -{\color{#58F}x}
> \end{align}
>  &
>  \displaystyle 
>  
>  \frac{\partial f}{\partial {\color{#58F}x}} = \frac{\partial {\color{#F05}d}}{\partial {\color{orange}c}} \frac{\partial {\color{orange}c}}{\partial {\color{#A8F}b}} \frac{\partial {\color{#A8F}b}}{\partial {\color{#0C8}a}} \frac{\partial {\color{#0C8}a}}{\partial {\color{#58F}x}} = -\frac{2}{{\color{orange}c}^{2}}\cdot \cos {\color{#A8F}b}\cdot e^{{\color{#0C8}a}}\cdot -1
> \end{array}$$
> Now that we have the local derivatives, we can give an input ($x=-4.499$), and compute the output following the computation graph:
> ```mermaid
> flowchart LR
> 	x:::xClass --> a:::aClass --> b:::bClass --> c:::cClass --> d:::dClass
> 	classDef xClass stroke:#58F
> 	classDef aClass stroke:#0C8
> 	classDef bClass stroke:#A8F
> 	classDef cClass stroke:#F90
> 	classDef dClass stroke:#F05
> ```
> $$
> \begin{array}{cc}
> \begin{align}
> {\color{#F05}d}(1) &= 2 \\
> {\color{orange}c}(90) &= 1 \\
> {\color{#A8F}b}(4.499) &= 90 \\
> {\color{#0C8}a}(-4.499) &= 4.499
> \end{align}
>  &
>  \displaystyle 
>  
>  \frac{\partial f}{\partial {\color{#58F}x}} =  -\frac{2}{{\color{orange}1}^{2}}\cdot \cos {\color{#A8F}90}\cdot e^{{\color{#0C8}4.499}}\cdot -1 = 0
> \end{array}$$

- Computing the gradient for ${\color{orange}v_{2}}$ would look like this: $$\begin{align}l &= ({\color{#F05}y}-t)^{2} \\ {\color{#F05}y} &= {\color{orange}v_{1}}h_{1}+{\color{orange}v_{2}}h_{2}+{\color{orange}v_{3}}h_{3}+{\color{#58F}b_{h}} \\ \\ \frac{\partial l}{\partial {\color{orange}v_{2}}} &= \frac{\partial l}{\partial {\color{#F05}y}}\frac{\partial {\color{#F05}y}}{\partial {\color{orange}v_{2}}} \\ &= 2({\color{#F05}y}-t) \cdot h_{2} \\ \\ {\color{orange}v_{2}} &\leftarrow {\color{orange}v_{2}}- \eta \cdot 2({\color{#F05}y}-t)\cdot h_{2}\end{align}$$

- because of the graph structure, we can reuse computed terms (take as an example the gradient for ${\color{orange}w_{12}}$) $$\begin{align}{\color{orange}v_{2}} \leftarrow {\color{orange}v_{2}}- \eta \cdot &2({\color{#F05}y}-t)\cdot h_{2} \\ {\color{orange}w_{12}} \leftarrow {\color{orange}w_{12}}- \eta \cdot &2({\color{#F05}y}-t)\cdot {\color{orange}v_{2}} \cdot h_{2} (1 - h_{2}) \cdot x_{1}\end{align}$$

# Back-propagation
- back-propagation: we can compute all derivatives in a single walk down the computation graph
	- values are *circle nodes* and computations are *diamond nodes*

![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0034anim1.svg)
$$\begin{align}
l &= (y- t)^{2} \\
y &= v_{1}h_{1}+v_{2}h_{2}+v_{3}h_{3}+c \\
h_{1} &= \frac{1}{1+\text{exp}(-k_{1})} \\
k_{1} &= w_{11}x_{1}+w_{21}x_{2}+b_{1}
\end{align}$$

> [!info] Step of back-propagation
> 1. Do a forward pass: compute the output and loss given the inputs
> 2. Start at the top (output) of your computation graph: The computation graph should always have a single output
> 3. Compute the derivative for every node: That is, the derivative of the output with respect to the node
> 4. Work you way down: As we will see, this makes the computation entirely local

- the derivative of $l$ wrt. $y$ tells use how we would update $y$ if we could do so directly

> [!tip]-
> The derivative of the loss wrt to the input is always $$\frac{\partial l}{\partial \text{input}}= \underbrace{\frac{\partial l}{\partial \text{output}}}_{\text{already computed}} \overbrace{\frac{\partial \text{output}}{\partial \text{input}}}^{\begin{array}{c} \text{local derivative for} \\ \text{current computation}\end{array}}$$
> ![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0039anim0.svg)

- for every input of every computation, we can work out the derivative in terms of the derivative for **the output**

# Maximum margin loss
- a logistic regression model can split the data perfectly, however a new instance could be classified incorrectly -> **maximize distance to decision boundary** from each class (**maximum margin hyperplane**)
	- measure distance at a right angle to decision boundary
	- points closest to decision boundary called **support vectors**
	- **margin**: distance to support vectors (distance where the hyperplane $h$ intersects 0 and 1/-1)
- a decision boundary can be defined by infinitely many hyperplanes ![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0054anim3.svg)
	- we choose the hyperplane such that the positive support vector is 1 and -1 for the negative support vector ![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0056anim4.svg) ![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0060.svg)
	- **objective**: maximize "2x the size of the margin" such that $$\begin{align}w^{\top}x_{i}+b \geq 1 &\quad \text{for } x_{i}\in X^{P} \\ w^{\top}x_{i}+ b \leq -1 &\quad \text{for } x_{i}\in X^{N}\\ \Rightarrow y_{i}(w^{\top}x_{i}+b)\geq 1 &\quad \text{for all }x_{i}\end{align}$$
		- "2x the size of the margin" can be formalised as $\frac{2}{||w||}$ or $\frac{1}{2} ||w||^{2}$
	- *hard margin*: no points can be inside the margin
	- *soft margin*: a exceptions can be made 
		- for data with few outliers or non-linear data
		- minimize: $$\frac{1}{2}w^{\top}w + C\sum\limits_{i}p_{i}$$ such that: $$\begin{align}y_{i}(w^{\top}x_{i}+b)&\geq 1-p_{i}\quad \text{for all }x_{i} \\ p_{i}&\geq 0\end{align}$$
	- search algorithm automatically find fitting $p_{i}$

> [!example]- Soft margin can provide a better fitting decision boundary on linearly separable data 
> ![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0073.svg)

- We can formulate the objective only in terms of $w$ -> allows gradient descent and neural networks/deep learning: $$p_{i}=\text{max}(0,1-y_{i}(w^{\top}x_{i}+b))$$ -> minimize: $$\frac{1}{2}w^{\top}w+C\sum\limits_{i}\text{max}(0,1-y_{i}(w^{\top}x_{i}+b))$$
	- with this the constraints are *always true* giving us an **unconstrained optimization problem** which we can use as a loss function on any model (called **L1-SVM**)

> [!summary] Classification losses
> - Least squares loss
> - Log loss / logistic regression
> - SVM loss

> [!summary]- Loss functions
> ![|500](https://mlvu.github.io/beyondlinear/32.Linear.key-stage-0080.svg)
# Support Vector Machines
## 🔹 Introduction

Support Vector Machines (**SVMs**) are powerful models for **classification and regression** that extend **linear models** to handle **non-linearly separable data** using **kernel tricks**.

The main ideas behind **SVMs**:  
- **Maximize margin** between classes.  
- **Use kernels** to map data to higher dimensions.  
- **Handle non-separable cases** with **slack variables**.

---

## 📌 Recap: Linear SVMs

A **hard-margin SVM** (for linearly separable data) finds a decision boundary:

$$w^{\top}x+b=0$$

The goal is to **maximize the margin** $\frac{2}{\lVert w\rVert}$, leading to the **optimization problem**:

$$\underset{w,b}{\min} \frac{1}{2} {\lVert w\rVert}^2$$

subject to:

$$y_i (w^T x_i + b) \geq 1, \quad \forall i$$

where:

- $w$ is the weight vector,
- $b$ is the bias,
- $y_i \in \{+1, -1\}$ are class labels.

> [!warning] **Limitations of Linear SVMs**
> 
> - Only works for **linearly separable** data.
> - Sensitive to **outliers**.
> - Can't model **complex decision boundaries**.

---

## 🔥 Soft-Margin SVM (Handling Overlap & Noise)

For **non-separable data**, we introduce **slack variables** $\xi_i$ to allow some misclassification:

$$y_i (w^T x_i + b) \geq 1 - \xi_i, \quad \forall i$$

The new **objective function** balances **margin size** and **misclassification**:

$$\min_{w, b, \xi} \frac{1}{2} \|w\|^2 + C \sum_{i=1}^{n} \xi_i$$

where:

- $C$ is a **regularization parameter** controlling the trade-off between **margin size** and **misclassification**.

> [!info] **Choosing $C$**
> 
> - **Large $C$** → fewer misclassifications (overfitting).
> - **Small $C$** → larger margin with more misclassifications (better generalization).

---

## 🎭 Non-Linearly Separable Data: The Kernel Trick

If data **isn't linearly separable**, we map it to a **higher-dimensional space** using a **feature transformation** $\phi(x)$:

$$\Phi: \mathbb{R}^d \to \mathbb{R}^{D}, \quad D \gg d$$

> [!question] **Why not explicitly transform data?**  
> Computing $\Phi(x)$ directly is **computationally expensive**. Instead, we use **Kernels** to compute dot products **efficiently** in the transformed space.

### Kernel Trick

Instead of computing $\Phi(x)$ explicitly, we compute:

$$K(x, x') = \langle \Phi(x), \Phi(x') \rangle$$

where $K(x, x')$ is a **kernel function** that measures similarity.

> [!tip] **Think of kernels as a shortcut!**  
> Instead of transforming data, we compute the dot product in the **high-dimensional space directly**.

---

## 🔑 Common Kernel Functions

### 1️⃣ Polynomial Kernel

$$K(x, x') = (x^T x' + c)^d$$

- Allows learning **curved decision boundaries**.
- $d$ controls the degree of the polynomial.
- Larger $d$ makes the boundary more flexible.

### 2️⃣ Radial Basis Function (RBF) Kernel

$$K(x, x') = \exp \left(-\gamma \|x - x'\|^2 \right)$$

- Maps data to an **infinite-dimensional space**!
- **$\gamma$** controls spread → small $\gamma$ means smoother decision boundary.

### 3️⃣ Sigmoid Kernel (Used in Neural Networks)

$$K(x, x') = \tanh(\alpha x^T x' + c)$$

- Inspired by **neural networks**.
- Less commonly used in practice.

> [!info] **Which kernel should I choose?**
> 
> - **Linear Kernel** → Use when data is roughly linear.
> - **Polynomial Kernel** → When data has polynomial-like structure.
> - **RBF Kernel** → Default choice for most problems.

---

## ⚙️ **Dual Formulation of SVMs**

Instead of solving in **primal space**, we reformulate in terms of **Lagrange multipliers** $\alpha_i$:

$$\max_{\alpha} \sum_{i=1}^{n} \alpha_i - \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} y_i y_j \alpha_i \alpha_j K(x_i, x_j)$$

subject to:

$$0 \leq \alpha_i \leq C, \quad \sum_{i=1}^{n} \alpha_i y_i = 0$$

where $\alpha_i$ controls how much influence each support vector has.

### **Decision Function**

Once trained, classification is done via:

$$f(x) = \sum_{i=1}^{n} \alpha_i y_i K(x_i, x) + b$$

> [!note] **Key Insights**
> 
> - Only **support vectors** have $\alpha_i > 0$ (most points don’t matter).
> - Kernel trick is **used here** to compute similarities efficiently.

---

## 🏁 **Summary**

✅ **SVMs maximize margin** to improve generalization.  
✅ **Soft-margin SVM** allows handling **overlapping classes**.  
✅ **Kernel trick** enables modeling **non-linearly separable** data.  
✅ **RBF kernel is the most widely used**.  
✅ **Only support vectors determine decision boundary**.

---

## 🎯 **Final Thoughts**

SVMs are **versatile and powerful**, especially for small to medium-sized datasets. However, they can be **computationally expensive** for large datasets, where **deep learning** may be more practical.


---
References:https://mlvu.github.io/beyondlinear/
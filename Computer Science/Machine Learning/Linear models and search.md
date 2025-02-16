---
Tags: 
Created: 2025-02-09 17:11:35
---
(Links:: [[Machine Learning]])
# Linear Regression
- **linear regression model** is a simple linear function that maps features to the target
	- works when the feature $x$ and target $t$ are numbers

> [!info]- Notation
> - scalar: $\mathscr{x},\mathscr{y}, \mathscr{z}$
> - vector: $\mathbf{x}, \mathbf{y}, \mathbf{z}$
> - matrix: $\mathbf{X}, \mathbf{Y}, \mathbf{Z}$
> - scalar element of a vector $\mathbf{x}$: $\mathscr{x}_{i}$
> - scalar element of a matrix $\mathbf{X}$: $X_{ij}$
> $$\mathbf{x}=\begin{pmatrix}\mathscr{x}_{1} \\ \mathscr{x}_{2} \\ \vdots \\ \mathscr{x}_{m}\end{pmatrix}$$
> $\mathbf{x}_{i}$: Instance $i$ in the data
> $\mathscr{x}_{j}$: Feature $j$ (of some instance)
> $X_{ij}$: Feature $j$ of instance $i$

A standard linear regression model with *one* or *$n$* parameters can be described by the following formulas respectively ($\mathscr{w}$: weight, $b$: bias): 
$$f_{\mathscr{w},b}(\mathbf{x})=\mathscr{wx}_{1}+b$$ 
<!-- $$f_{w_{1},w_{2},b}(\mathbf{x})=w_{1}\mathscr{x}_{1}+w_{2}\mathscr{x}_{2}+b$$ -->
$$\begin{align}
f_{\mathbf{w},b}(\mathbf{x})&=\mathscr{w}_{1}\mathscr{x}_{1}+\mathscr{w}_{2}\mathscr{x}_{2}+\mathscr{w}_{3}\mathscr{x}_{3}+\cdots + b \\
&= \mathbf{w}^{\top}\mathbf{x}+b \\ \\
\text{with } \mathbf{w}&= \begin{pmatrix}\mathscr{w}_{1}\\ \vdots\\ \mathscr{w}_{n}\end{pmatrix} \text{ and } \mathbf{x}=\begin{pmatrix}\mathscr{x}_{1}\\ \vdots\\ \mathscr{x}_{n}\end{pmatrix}
\end{align}$$
We can construct linear regression model for a prediction with the [[dot product]] between a vector of *weights* $\mathbf{w}$ and one of *features* $\mathbf{x}$. The values in $\mathbf{w}$ are *positive* if they should contribute to the respective feature, and *negative* if the opposite predicts the feature. In addition, the *larger* the weight, the more it contributes to a certain feature.
Now we use the [[Mean Squared Error Loss]] function to determine if a model performs well: 
$$\begin{align}
loss_{X,\top}(p) &= \frac{1}{n} \sum_{j}\big(f_{p}(x_{j})-t_{j}\big)^{2} \\
loss_{X,\top}(\mathbf{w},b) &= \frac{1}{n}\sum\limits_{j}(\mathbf{w}^{\top}\mathbf{x}_{j}+b-t_{j})^{2}
\end{align}$$
- it takes a *model* as an argument and maps it to a loss value
- depending on the difference between the model ($f_{p}(\mathbf{x}_{j})$) and the actual data ($t_{j}$), if it is large, then so will the loss value be
- *squaring* the **residuals** ensures negative and positive values don't cancel out and that larger errors have a larger effect

# Searching for a good model
- **Loss surface**: a plot of all loss functions in the *model space*
  We can describe points with the smallest loss as being brighter in the image
- To **optimize** the loss function we try to find a point $p$ where the function is at its *minimum* described through this function $$\hat p=\underset{p}{\text{ arg min }} \textbf{loss}_{X,\top}(p)$$
  $p$: model parameters
- There are two types of minimum
	- *local minimum*: all nearby points are higher
	- *global minimum*: the actual point we're trying to aim for (lowest place on the surface)
- optimize for the *test data* not on training data otherwise this leads to **overfitting**

> [!info] Random Search
> 1. Start with random point $p$ in the *model space*
> 2. Loop:
> 	1. Pick a random point $p'$ close to $p$
> 	2. if($loss(p') < loss(p)$): $p\leftarrow p'$

You can imagine random search as blindly picking a direction, and testing if a certain distance $r$ in that direction is lower. If so update to the new position otherwise try again.
![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0028.svg)
There is an issue: random search is susceptible to local minima, and will never escape. To help against this we use **simulated annealing**:
1. Pick a random point **$p$** in the *model space*
2. Loop:
	1. Pick a random point $p'$ close to $p$
	2. if $loss(p') < loss(p)$: $p \leftarrow p'$
	3. else: 
	   with probability $q$: $p\leftarrow p'$

In cases where it goes uphill, there's a slight change we take this path.
> [!warning] Local minima aren't always bad solutions

- the space of linear models is **continuous**: we don't always have another model "in between" any two given models
	- In in these cases, random search and simulated annealing will work
- **parallel search**: run random search multiple times for higher likely hood of finding global minima
	- communication between parallel runs can guide searches more (**population methods**) and provide *faster coverage*
		- *Pro*: powerful and easy to parallelise
		- *Con*: slow/expensive for complex models and difficult to tune
- **Evolutionary algorithms** (population method):
1. Start with a *population* of $k$ models
2. Loop:
	1. Rank the population by loss
	2. Remove the half with the worst loss
	3. "Breed" a new population of $k$ models
	4. Optional: Add a little noise to each child

![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0042anim2.svg)

- **Black box optimization**: methods where we only compute loss function
	- often info on model to improve search
# Gradient Descent
- Instead of picking direction arbitrarily (as with random search), look in $k$ different directions and choose the position with the lowest value:
> [!info] Branching search
> 1. Pick a random point $p$ in the model space
> 2. Loop:
> 	1. Pick $k$ random points $\{p_{i}\}$ close to $p$
> 	2. $p' \leftarrow \text{argmin}_{p_{i}}\, loss(p_{i})$
> 	3. if $loss(p')<loss(p)$: $p\leftarrow p'$

![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0049.svg)

![[Branch Search.svg|500]]

%%![[Branch Search|500]] %%

- If the model space is **continuous** then we don't need to optimize the loss function but we can simply derive it using calculus (taking the [[Derivative]]):
	1. Find the *direction* in which the loss drops most quickly
	2. This direction is the opposite of the **gradient**
	3. Gradient descent takes small steps in this direction in order to find the minimum of a function
- The **tangent hyperplane** intersects the function at one point
	- It's derivative is called the **gradient** which is described by a vector 
	- The vectors' values dictate the slope in different directions of higher dimensions
	  ![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0054anim1.png)
- For a function with $n$ variables (higher dimension functions), we need to take $n$ derivatives, which we can compute using **partial derivates**
	- When taking the derivative with respect one variable $x$, you must treat all other variables as constants
	- The gradiant vector for two dimensional functions is defined as $$\nabla f(\mathscr{x},\mathscr{y})=\left(\frac{\partial f}{\partial \mathscr{x}},\frac{\partial f}{\partial \mathscr{y}}\right)$$ and thus the tangent hyperplane can be defined as $$g(\mathbf{x})=\nabla f(p)^{\top}\mathbf{x}+c$$
- To calculate the *direction of steepest descent*, we can do the following:
	- We remove $c$ since the position (or translation) doesn't influence the direction of a vector
	- From the definition of the [[dot product]] it follows that $$\nabla f(p)^{\top}\mathbf{x} = ||\nabla f(p)||\;||\mathbf{x}|| \cos \alpha$$ Since we only care about the direction, and not the magnitude $$||\nabla f(p)||\;||\mathbf{x}|| \cos \alpha= ||\nabla f(p)|| \cos\alpha\qquad \text{where } ||\mathbf{x}||=1$$ 
	- $\cos \alpha$ is largest when $\alpha=0$ which is dependent on how much $\mathbf{x}$ and $\nabla f(p)$ point in the same direction. 
	- Thus the direction where the hyperplane $g(\mathbf{x})$ grows quickest is $\nabla f(p)$, and it's steepest descent is $-\nabla f(p)$

> [!summary]
> - The tangent hyperplane of a function $f$ *approximates* the function $f$ locally
> - The gradient $\nabla f$ gives the *slope* of this tangent hyperplane
> - The vector expressing the slope of a hyperplane is also the *direction of steepest ascent* on that hyperplane
> - The opposite vector is the **direction of steepest descent**
> - **Conclusion**: To move to a lower point of $f$, we can compute the gradient and take a small step in the opposite direction

> [!info] Gradient Descent Algorithm
> 1. Pick a random point $p$ in the model space
> 2. Loop: $p \leftarrow p-\upeta \nabla loss(p)$ (We set the **learning rate** $\upeta$ somewhere between 0.0001 and 0.1)

> [!example]-
> $$\begin{align}
> loss(\mathscr{w},b) &=\frac{1}{n}\sum\limits_{i}(\mathscr{wx}_{i}+b-t_{i})^{2} \\
> \nabla loss(\mathscr{w},b) &=
> \begin{pmatrix} 
> \frac{\partial loss(\mathscr{w},b)}{\partial \mathscr{w}} \\
> \frac{\partial loss(\mathscr{w},b)}{\partial b}
> \end{pmatrix}\tag{1}
> \end{align}$$
> $$\begin{align}
> \frac{\partial loss(\mathscr{w},b)}{\partial \mathscr{w}} &= \frac{\partial\frac{1}{n}\sum_{i}(\mathscr{wx}_{i}+b-t_{i})^{2}}{\partial \mathscr{w}} \\
> &= \frac{1}{n}\sum\limits_{i} \frac{\partial(\mathscr{wx}_{i}+b-t_{i})^{2}}{\partial \mathscr{w}} \\
> &= \frac{1}{n}\sum\limits_{i} \frac{\partial(\mathscr{wx}_{i}+b-t_{i})^{2}}{\partial(\mathscr{wx}_{i}+b-t_{i})} \frac{\partial(\mathscr{wx}_{i}+b-t_{i})}{\partial \mathscr{w}}\\
> &= \frac{2}{n}\sum\limits_{i} (\mathscr{wx}_{i}+b-t_{i})\mathscr{x}_{i} \tag{2}\\
> \frac{\partial loss(\mathscr{w},b)}{\partial b} &= \frac{\partial\frac{1}{n}\sum_{i}(\mathscr{wx}_{i}+b-t_{i})^{2}}{\partial b} \\
> &= \frac{2}{n}\sum\limits_{i} (\mathscr{wx}_{i}+b-t_{i})\tag{3}\\
> \end{align}$$
> Inserting $(2)$ and $(3)$ into $(1)$ we get
> $$\begin{pmatrix} \mathscr{w} \\ b \end{pmatrix} \leftarrow
> \begin{pmatrix} \mathscr{w} \\ b \end{pmatrix} - \upeta
> \underbrace{
> \begin{pmatrix}
> \frac{2}{n}\sum_{i}(\mathscr{wx}_{i}+b-t_{i})\mathscr{x}_{i} \\
> \frac{2}{n}\sum_{i}(\mathscr{wx}_{i}+b-t_{i})
> \end{pmatrix}\;}_{\text{gradient}}
> $$
> ![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0067.png)

- The algorithm *always* computes the optimal step and takes it
- When getting closer to the minimum, the magnitude decreases and thus also the **step size** -> we don't *overshoot* the optimum
- still susceptible to *local minima*
- learning rate is dependent on dataset and model -> trial and error to find good value

> [!summary] Gradient Descent
> Gradient descent requires a *continuous model space*, where the loss function is *smooth* so that it can compute the gradient vector. This method is *very fast*, requires *little memory*, and is *very accurate*. However by itself it cannot escape *local minima*.

> [!warning]- Linear Regression can by solved *analytically* and doesn't require *searching*
> Simply set the partial derivates to 0 and solve for $\mathscr{w}$ and $b$.
# Gradient Descent and Classification
- a decision boundary is defined using the function $f_{\mathbf{w},b}(\mathbf{x})=\mathbf{w}^{\top}\mathbf{x}+b$ where
	- if $\mathbf{w}^{\top}\mathbf{x} + b>0$: $\mathbf{x}$ is in a class
	- if $\mathbf{w}^{\top}\mathbf{x} + b<0$: $\mathbf{x}$ is in the other class
	- In this case, the first is in the blue class, the other in the red class
	![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0080.svg)
	![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0081.svg)
	- For binary classification $\mathbf{w}$ is always *perpendicular* to the decision boundary
- If we instead of using the [[Mean Squared Error Loss]], we simply use as a loss function the number of misclassified examples, we get a loss surface with *discrete* changes: ![|500](https://mlvu.github.io/linear/12.Linear1.key-stage-0084.svg)
	- Previous algorithms won't work nicely with this!

> [!danger] Sometimes your **loss function** should not be the same as your **evaluation function**!

- Using least-squares loss, we turn the [[Introduction to Machine Learning#Classification|classification]] problem into a [[Introduction to Machine Learning#Regression|regression]] problem: $$loss(\mathbf{w},b)=\underbrace{\sum\limits_{i\in\text{pos}}(\mathbf{w}^{\top}\mathbf{x}_{i}+b-1)^{2}}_{\text{loss for instances in pos class}}+\underbrace{\sum\limits_{i\in\text{neg}}(\mathbf{w}^{\top}\mathbf{x}_{i}+b+1)^{2}}_{\text{loss for instances in neg class}}$$
- We now use [[Mean Squared Error Loss]] to train a regression model

> [!warning] The optimum under the loss function may not always perfectly separate the classes

---
References: https://mlvu.github.io/linear/
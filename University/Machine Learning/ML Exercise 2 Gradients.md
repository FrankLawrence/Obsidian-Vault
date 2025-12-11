---
title: Week 2 Answers
tags: [math, machine-learning, linear-algebra, calculus]
---

# Partial derivatives

> [!question]- Question 1
> To find the $a$ and $b$ for which $f(a,b)$ is minimal, we set the partial derivatives to zero. Fill in the blanks in this derivation:
> $$\begin{align}
> 2b - a &= 0 \\
> b &= a/2\\
> b &= \dots \\
> \dots &= \dots \\
> b &= - \frac{1}{11}
> \end{align}$$
> $$\begin{align*}
> 6a - b + 1 &= 0\\
> a &= \frac{b - 1}{6}\\
> a &= \dots = - \frac{2}{11}
> \end{align*}$$
> > [!info]- Solution
> > $$\begin{align}
> > 2b - a &= 0 \\
> > b &= a/2\\
> > b &= {\color{#F08}\left(\frac{b-1}{6}\right)/2} \\
> > {\color{#F08}12b} &= {\color{#F08}b - 1} \\
> > b &= - \frac{1}{11}
> > \end{align}$$
> > $$\begin{align*}
> > 6a - b + 1 &= 0\\
> > a &= \frac{b - 1}{6}\\
> > a &= {\color{#F08}- \left(\frac{12}{11}\right)/6} = - \frac{2}{11}
> > \end{align*}$$

## Rules
$$\begin{align}
\frac{{\color{#888}\partial} c}{{\color{#888}\partial} x} &=0 &&\text{the \textit{constant} rule} \\
\frac{{\color{#888}\partial} x^{{\color{#F08}n}}}{{\color{#888}\partial} x} &={\color{#F08}n}x^{{\color{#F08}n}-1} &&\text{the \textit{exponent} rule} \\
\frac{{\color{#888}\partial} x}{{\color{#888}\partial} x} &= 1 &&\text{(follows from the exponent rule)} \\
\frac{{\color{#888}\partial} {\color{#F08}c}f(x)}{{\color{#888}\partial} x} &= {\color{#F08}c}\frac{{\color{#888}\partial} f(x)}{{\color{#888}\partial} x} &&\text{the \textit{constant factor} rule} \\
\frac{{\color{#888}\partial} {\color{#58F}f(x)}+{\color{#0C8}g(x)}}{{\color{#888}\partial} x} &= \frac{{\color{#888}\partial} {\color{#58F}f(x)}}{{\color{#888}\partial} x} + \frac{{\color{#888}\partial} {\color{#0C8}g(x)}}{{\color{#888}\partial} x} &&\text{the \textit{sum} rule} \\
\frac{{\color{#888}\partial} {\color{#58F}f({\color{#0C8}g(x)})}}{{\color{#888}\partial} x} &= \frac{{\color{#888}\partial} {\color{#58F}f({\color{#0C8}g(x)})}}{{\color{#888}\partial} {\color{#0C8}g(x)}} + \frac{{\color{#888}\partial} {\color{#0C8}g(x)}}{{\color{#888}\partial} x} &&\text{the \textit{chain} rule} \\
\end{align}$$

> [!question] Question 2
> To practice, let’s take the derivative of $(x + y + z)^{2}$ with respect to $y$. Fill in the blanks.

---
# Linear Regression

$$\begin{array}{l|l}
\text{age}(a, \mathrm{months}) & \text{height}(h,\mathrm{cm}) \\ \hline
a_{1} = 0 & h_{1}= 30 \\
a_{2} = 2 & h_{2}= 40 \\
a_{3} = 4 & h_{3}= 50 \\
\end{array}$$

Our model $f$ is a linear function described by two parameters: the slope $s$ and the intercept $b$ with feature $a$ and target $h$: $$f_{s,b}(a)=sa+b$$
$$\text{loss}(s,b)=\frac{1}{2} \sum\limits_{i}\big({\color{#F08}f_{s,b}(a_{i})}-h_{i}\big)^{2} = \frac{1}{2} \sum\limits_{i} \left({\color{#F08}sa_{i}+b}-h_{i}\right)^{2}$$

> [!question] Question 3
> For $s=1$ and $b=0$, and the data given above, what is the loss?
> 

> [!question] Question 4
> The term $f(a_{i})-h_{i}$ represents the difference between our model’s prediction and the observed value (the *residual*). Per example, this is a good measure of the error. Why do we not just sum these, and check how far it is from 0? Why square it first, and *then* sum?
> 
> **If we summed them, one big positive error could cancel out against one big negative error, giving the false impression that the model is highly ac- curate. We need to square the errors before summing them. The choice for square instead of another approach (like taking the absolute value, or raising to some other power) is more subtle. Squaring emphasizes the loss of large errors compared to the absolute value.
> It turns out that if we assume that the data is linear, but with added [[Gaussian noise]], optimizing for the sum-of-squared errors is equivalent to optimizing likelihood (we will discuss this later in the probability lectures.)**

> [!question] Question 5

> [!question] Question 6 
> For many loss functions, setting the gradients equal to zero results in a system of equations that cannot be solved analytically. In that case we will have to *search*. We can still use the gradient though, in an algorithm called *gradient descent*. Starting with the parameter values $s = 1$ and $b = 0$, describe one step of the gradient descent algorithm (with learning rate 0.01).
> 
> **For these parameters, the gradient is** 
> $$\begin{align}
> &\left( {\color{#F08}s\sum\limits_{i} {a_{i}}^{2} + b \sum\limits_{i} a_{i} - \sum\limits_{i} a_{i}h_{i}}, {\color{#58F}s\sum\limits_{i}a_{i}+bn - \sum\limits_{i}h_{i}} \right) \\
> &= \left( {\color{#F08}\sum\limits_{i} {a_{i}}^{2} - \sum\limits_{i} a_{i}h_{i}}, {\color{#58F}\sum\limits_{i}a_{i}- \sum\limits_{i}h_{i}} \right) \\
> &= ({\color{#F08}4+16-(2\cdot 40+4\cdot 50)}, {\color{#58F}6-120}) \\
> &= ({\color{#F08}-260},{\color{#58F}-114})
> \end{align}$$
> **For gradient descent we pick the opposite direction (since the gradient points up), multiply by the learning rate, and add the result to the current parameters. Thus, the new parameters are:** $$\begin{align}
> \begin{bmatrix}s^{\text{new}} \\ b^{\text{new}}\end{bmatrix} &= \begin{bmatrix}s \\ b\end{bmatrix} - \eta\nabla \text{loss}(s,b) \\
> &= \begin{bmatrix}1 \\ 0\end{bmatrix} - 0.01 \begin{bmatrix}{\color{#F08}-260} \\ {\color{#58F}-114}\end{bmatrix} = \begin{bmatrix}3.6 \\ 1.14\end{bmatrix}
> \end{align}$$

> [!question] Question 7
> Set the two derivatives found above equal to zero, and solve to obtain expressions for the optimal model. Make sure that your expression for s does not depend on b, so that the solution can actually be computed. To simplify notation, it can be helpful to use the following conventions for the data means and other statistics:
> > [!info]- Abbreviations
> > $$\begin{align}
> > \overline a &= \frac{1}{n} \sum\limits_{i} a_{i} \\
> > \overline h &= \frac{1}{n} \sum\limits_{i} h_{i} \\
> > \overline {a^{2}} &= \frac{1}{n} \sum\limits_{i} {a_{i}}^{2} \\
> > \overline {h^{2}} &= \frac{1}{n} \sum\limits_{i} {h_{i}}^{2} \\
> > \overline {ah} &= \frac{1}{n} \sum\limits_{i} a_{i}h_{i} \\
> > \end{align}$$
> 
> > [!info]- Solution
> > $$\begin{align}
> > s\sum\limits_{i} {a_{i}}^{2} + b \sum\limits_{i} a_{i} - \sum\limits_{i} a_{i}h_{i} &= 0 \\
> > s\sum\limits_{i} {a_{i}}^{2} &=  - b \sum\limits_{i} a_{i} + \sum\limits_{i} a_{i}h_{i} \\
> > s &= - b \frac{\sum_{i} a_{i}}{\sum_{i} {a_{i}}^{2}} + \frac{\sum_{i} a_{i}h_{i}}{\sum_{i} {a_{i}}^{2}} \\
> > s &= - b \frac{\frac{1}{n}\sum_{i} a_{i}}{\frac{1}{n}\sum_{i} {a_{i}}^{2}} + \frac{\frac{1}{n}\sum_{i} a_{i}h_{i}}{\frac{1}{n}\sum_{i} {a_{i}}^{2}} \\
> > s &= -b \frac{\overline a}{\overline{a^{2}}} + \frac{\overline{ah}}{\overline{a^{2}}} \tag 1 \\ \\
> > 
> > s\sum\limits_{i}a_{i}+bn - \sum\limits_{i}h_{i} &= 0 \\
> > bn &= \sum\limits_{i}h_{i} - s\sum\limits_{i}a_{i} \\
> > b &= \frac{1}{n}\sum\limits_{i}h_{i} - s\frac{1}{n} \sum\limits_{i}a_{i} \\
> > b &= \overline{h} - s \overline{a} \tag 2 \\ \\
> > 
> > s &= - (\overline{h} - s \overline{a}) \frac{\overline a}{\overline{a^{2}}} + \frac{\overline{ah}}{\overline{a^{2}}} \qquad \text{fill equation $(2)$ into $(1)$} \\
> > s &= - \frac{\overline{h}\overline{a}}{\overline{a^{2}}} + s \frac{\overline{a}^{2}}{\overline{a^{2}}} + \frac{\overline{ah}}{\overline{a^{2}}} \\
> > s \left(1 - \frac{\overline{a}^{2}}{\overline{a^{2}}}\right) &= - \frac{\overline{h}\overline{a}}{\overline{a^{2}}} + \frac{\overline{ah}}{\overline{a^{2}}} \\
> > s &= \frac{\overline{ah} - \overline{a}\overline{h}}{\overline{a^{2}}-\overline{a}^{2}}
> > \end{align}$$

> [!question]- Question 8
> Fill in the values from the data set, and compute the optimal parameters for this data. Can you explain what the parameters s and b mean? That is, what can they tell us about the baby we’ve measured?
> **Filling in the data gives us $\overline{ah}=\frac{280}{3}, \overline{a}=2, \overline h = 40, \overline{a^{2}}=\frac{20}{3}$. This gives us** $$s = \frac{\frac{280}{3}-2\cdot 40}{\frac{20}{3}-4}=\frac{\frac{40}{3}}{\frac{8}{3}}=5$$ $$b = 40 - 5 \cdot 2 = 30$$ 
> **This tells us that this baby grows about 5 cm per month and its size at birth was 30cm**

---
# Additional Resources

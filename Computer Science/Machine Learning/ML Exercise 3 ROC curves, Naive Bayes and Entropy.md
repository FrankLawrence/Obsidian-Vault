---
title: Week 3 Answers
cssclasses:
  - remove-left-border-embed
---

# Exam Questions
> [!question]+ Question 1
> What is the advantage of gradient descent over random search?
> 1. In gradient descent, parallel searches are allowed to communicate.
> 2. Gradient descent is less likely to get stuck in local minima.  
> 3. Gradient descent computes the direction of steepest descent, random search approximates it.  
> 4. Gradient descent is easier to parallelise.

> [!question]+ Question 2
> Why is accuracy a bad loss function to use in gradient descent?
> 1. It is expensive to compute.
> 2. It makes the gradient zero almost everywhere.  
> 3. It is unreliable in situations with high class imbalance. 
> 4. The confidence interval is high on small test sets.

> [!question]+ Question 3
> I’m performing spam classification. I represent each email by three numbers: how often the word pill occurs, how often the word hello occurs and how often the word congratulations occurs. What are these three attributes called?
> 1. The instances  
> 2. The classes  
> 3. The features  
> 4. The principal components

Here we see the derivation of the gradient of the squared-error loss for linear regression.
$$\begin{align}
\frac{{\color{#888}\partial} \frac{1}{2} \sum_{i} (f(x_{i})-y_{i})^{2}}{{\color{#888}\partial} b} &= \frac{1}{2} \frac{{\color{#888}\partial} \sum_{i} (x_{i}w+b-y_{i})^{2}}{{\color{#888}\partial} b} \tag 1 \\
&= \frac{1}{2} \sum\limits_{i} \frac{{\color{#888}\partial} (x_{i}w+b-y_{i})^{2}}{{\color{#888}\partial} b} \tag 2 \\
&= \frac{1}{2} \sum\limits_{i} \frac{{\color{#888}\partial} (x_{i}w+b-y_{i})^{2}}{{\color{#888}\partial}(x_{i}w+b-y_{i})} \frac{{\color{#888}\partial} (x_{i}w+b-y_{i})}{{\color{#888}\partial} b} \tag 3 \\
&= \sum\limits_{i} (x_{i}w+b-y_{i}) \frac{{\color{#888}\partial} (x_{i}w+b-y_{i})}{{\color{#888}\partial} b} \tag 4 \\
&= \cdots \tag 5 \\
\end{align}$$

> [!question]+ Question 4
> To get from line $(1)$ to line $(2)$, we use the
> 1. Chain rule
> 2. Product rule
> 3. Exponent rule
> 4. Sum rule

> [!question]+ Question 5
> To get from line $(2)$ to line $(3)$, we use the
> 1. Chain rule
> 2. Product rule
> 3. Exponent rule
> 4. Sum rule

> [!question]+ Question 6
> In line $5$, the correct result is
> 1. $\sum_{i} ({x_{i}}^{2}w+b-y_{i})$
> 2. $\sum_{i} x_{i}(x_{i}w+b-y_{i})$
> 3. $\sum_{i} (x_{i}w+b-y_{i})$
> 4. $\sum_{i} (x_{i}w+b-y_{i})^{2}$

## Solutions
1. 3: Gradient descent computes the direction of steepest descent, random search approximates it.
2. 2: It makes the gradient zero almost everywhere.
3. 3: The features
4. 4: Sum rule
5. 1: Chain rule
6. 3: $\sum_{i} (x_{i}w+b-y_{i})$

---

# ROC Curves and confusion matrices
We use the following classification dataset:
$$\begin{array}{cccc}
& x_{1} & x_{2} & \text{label} \\ \hline
a & 1 & 0 & {\color{#F08}\text{Neg}} \\
b & 2 & 0 & {\color{#0C8}\text{Pos}} \\
c & 3 & 0 & {\color{#0C8}\text{Pos}} \\
d & \frac{1}{2} & 1 & {\color{#0C8}\text{Pos}} \\
e & 2 & 2 & {\color{#0C8}\text{Pos}} \\
f & 0 & 3 & {\color{#F08}\text{Neg}} \\
g & 1 & 3 & {\color{#F08}\text{Neg}} \\
h & 2 & 3 & {\color{#F08}\text{Neg}} \\
\end{array}$$

## Linear Classifier

> [!question]+ Question 1
> As a first classifier, $c_{\color{orange}\text{lin}}$, we will use a diagonal line crossing all the points where $x_1 = x_2$. Points above this line will be classified as ${\color{#F08}\text{Neg}}$, points below or on the line as ${\color{#0C8}\text{Pos}}$. How do we describe this classifier mathematically? 
> $$
>  c_{\color{orange}\text{lin}}(x_{1},x_{2}) = 
> \begin{cases}
> {\color{#0C8}\text{Pos}} & \text{if } x_{1} \geq x_{2} \\
> {\color{#F08}\text{Neg}} & \text{otherwise}
> \end{cases}$$

> [!question]+ Question 2
> Let $c_{\color{orange}2}$ be the classifier whose decision boundary crosses the points $(0, 1)$ and $(1, 1.5)$, with the class Neg above the line. Draw this line first. How do we define this classifier?
> $$
>  c_{\color{orange}2}(x_{1},x_{2}) = 
> \begin{cases}
> {\color{#0C8}\text{Pos}} & \text{if } x_{2} - \frac{1}{2}x_{1} - 1 \leq 0\\
> {\color{#F08}\text{Neg}} & \text{otherwise}
> \end{cases}$$

> [!question]+ Question 3
> We will compare the linear classifier $c_{\color{orange}\text{lin}}$ with a *decision tree* classifier, $c_{\color{orange}\text{tree}}$: 
> ![[Decision Tree ML Exercise 3]]
> If the inequality on the node is true, move right, otherwise move left.
> Label the leaves $A$, $B$, $C$, and $D$ with classes.
> $$A = {\color{#0C8}\text{Pos}} \quad B = {\color{#0C8}\text{Pos}} \quad C = {\color{#F08}\text{Neg}} \quad D = {\color{#0C8}\text{Pos}} $$


> [!question]+ Question 4

> [!question]- Give the confusion matrices for classifiers $c_{\color{orange}\text{lin}}$ and $c_{\color{orange}\text{tree}}$
> $$\begin{array}{cc|cc}
> c_{\color{orange}\text{lin}} & & \text{predicted} \\
> & & \text{Pos} & \text{Neg} \\
> \hline
> \text{true} & \text{Pos} & 3 & 1 \\
> & \text{Neg} & 1 & 3 \\
> \end{array}$$
> $$\begin{array}{cc|cc}
> c_{\color{orange}\text{tree}} & & \text{predicted} \\
> & & \text{Pos} & \text{Neg} \\
> \hline
> \text{true} & \text{Pos} & 4 & 0 \\
> & \text{Neg} & 2 & 2 \\
> \end{array}$$

> [!question]- Compute the accuracy, precision, recall, true positive rate and false positive rate
> 

> [!question]- How do we turn a linear classifier into a ranking classifier? Give the ranking that assigns $c_{\color{orange}\text{lin}}$ to the training data.
> We compute the distance to the classification boundary (the negative distance on one side, and the positive on the other), and rank points by this distance. For this course (the homework and the exam), you don’t actually need to compute the distance, it’s fine to draw the dataset and “measure” the distances. We’ll make sure that all examples allow this approach. 
> Here’s the sort of diagram you should end up with:
> ![[Pasted image 20250322161848.png|400]]

> [!question]- How do we turn a decision tree classifier into a ranking classifier? Give the ranking that $c_{\color{orange}\text{tree}}$ assigns to the training data.
> We group the points by the way the decision tree segments the feature space. We then assign each segment a class probability based on the relative frequencies of *training* data points. We sort the groups by class probability. Points within the same group are ranked equal.
> For $c_{\color{orange}\text{tree}}$ the ranking from negative to positive is $$({\color{#F08}f \; g}) \; ({\color{#F08}h} \; {\color{#0C8}e}) \; ({\color{#F08}a} \; {\color{#0C8}b \; c}) \; ({\color{#0C8}d})$$
> Brackets indicate equal rank.

> [!question]+ Question 9
> 1. Draw the coverage matrix of $c_{\color{orange}\text{lin}}$ and $c_{\color{orange}\text{tree}}$.
> $$\begin{array}{c|cccc}
> d & \cellcolor{red} & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} \\
> e & \cellcolor{red} & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} \\
> b & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} \\
> c & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} \\ 
> \hline
> c_{\color{orange}\text{lin}} & a & h & f & g
> \end{array}$$
> $$\begin{array}{c|cccc}
> d & \cellcolor{red} & \cellcolor{orange} & \cellcolor{green} & \cellcolor{green} \\
> e & \cellcolor{orange} & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} \\
> b & \cellcolor{orange} & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} \\
> c & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} & \cellcolor{green} \\ 
> \hline
> c_{\color{orange}\text{tree}} & a & h & f & g
> \end{array}$$
> 2. How many ranking errors does each classifier make on the training data? **$c_{\color{orange}\text{lin}}$ makes 2 ranking errors. $c_{\color{orange}\text{tree}}$ makes 2.5, counting each tie as half a ranking error.**
> 3. If we have class imbalance (one class has more instances than the other), how can we tell by the coverage matrix? **It will be non-square. The more stretched out, the higher the class imbalance.**

> [!question]+ How do we convert a coverage matrix to an ROC plot? How does the green area in a coverage matrix relate to the ROC plot? How do they differ?
> We can convert the coverage matrix to ROC space by normalizing the axes. That is, if we have a green cell at $(2, 3)$, counting from the bottom left, we can draw a point in ROC space at $({}^{2}\!/\!_{4}, {}^{3}\!/\!_{4})$. That is, we know we have a classifier that can achieve a true positive rate of 3/4 and a false negative rate of 2/4.

---

# Preliminaries Probability

> [!question]+ Question 11

> [!question]+ Question 12

> [!question]+ Assume that the probability that a given patient has diabetes is 0.01. We have a test for diabetes with a false positive rate of 0.05: if a patient has no diabetes, the test diagnoses it 5% of the time. The false negative rate is 0.1. You are a doctor, and you administer the test to a patient (knowing nothing else). The test says she has diabetes. What is the probability that she doesn’t?

--- 

# Naive Bayes

> [!question]+ Question 14

> [!question]+ Question 15

> [!question]+ Question 16

> [!question]+ Question 17

> [!question]+ Question 18

---

# Entropy

We define two probability distributions $p$ and $q$ on a set of four outcomes $\{a,b,c,d\}$: 
$$\begin{array}{cccc} p(a) & p(b) & p(c) & p(d) \\ \hline {}^{1}\!/\!_{4}& {}^{1}\!/\!_{4}&{}^{1}\!/\!_{4}&{}^{1}\!/\!_{4}\end{array}$$
$$\begin{array}{cccc} q(a) & q(b) & q(c) & q(d) \\ \hline {}^{1}\!/\!_{2}& {}^{1}\!/\!_{4}&{}^{1}\!/\!_{8}&{}^{1}\!/\!_{8}\end{array}$$

> [!question]- Could you simulate sampling from these distributions using coinflips as described in the lecture?
> Yes. $p$ can be simulated by assingin each unique sequence of two coinflips to one of the outcomes. For $q$, we assign the following sequences to each outcome:
> $$\begin{array}{cccc} q(a) & q(b) & q(c) & q(d) \\ \hline \textrm{H} & \textrm{TH} & \textrm{TTH} & \mathrm{TTT} \end{array}$$

> [!question]- Compute the entropy of $p$ and $q$
> 

> [!question]+ Question 21
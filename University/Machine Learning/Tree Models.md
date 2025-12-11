---
Tags: 
Created: 2025-03-02 23:43:37
---
(Links:: [[Machine Learning]])
# 🌳 Decision Trees

## Overview
Decision trees are models that recursively partition the feature space into subsets based on feature values, aiming to predict target variables effectively. Each internal node represents a decision based on a feature, and each leaf node represents a predicted outcome

## Training Process
1. **Starting Point**: Begin with the entire dataset at the root
2. **Splitting Criteria**: At each node, select the feature that best separates the data into distinct classes (non-uniform split)
	1. non-uniformity is measured with [[Probabilistic models#Entropy|entropy]], where the *higher* the entropy the *more* uniform the data
3. **Recursive Partitioning**: Apply the splitting process recursively to each subset until a stopping criterion is met (e.g., maximum depth, minimum samples per leaf)

## Information Gain
- We first use conditional entropy to find what the resulting entropy will be after a split

$$H(O \mid G) = \mathbb{E}_{g}H(O\mid G = g) = - \sum\limits_{g} p(g) H(O \mid G = g)$$

> [!example]
> $$p(\text{Outcome} = \text{ won} \mid \text{Genre } = \text{ Drama})$$
> $$H(O \mid G = d) = - \sum\limits_{o \in \{o,n,w\}} p(o \mid d) \log p(o \mid d)$$

- The **information gain** measures how much knowing the value of $G$ decreases the entropy of $O$ $$I_{O}(G)= H(O)- H(O\mid G)$$

## Example
Consider a dataset where each instance is a movie, and the target is to predict whether a movie won an Oscar, was nominated, or was overlooked. A trained decision tree might first split based on the genre, then on the rating, and so on, to classify each movie

> [!note] **Decision Tree Characteristics**
> - **Advantages**: Interpretable and easy to visualize
> - **Disadvantages**: Prone to overfitting; regularization can hurt performance

---
# 🔢 Numeric Features and Targets

- with numeric features **it** _does_ **make sense to split twice on the same feature**
- To reduce overfitting, we can **prune** a tree.
	- work backwards through tree and check if it works better without node -> we remove the node
## Regression Trees
When dealing with numeric (continuous) target variables, decision trees are adapted into regression tree. Instead of classifying, these trees predict continuous values by partitioning the feature space and fitting simple models (like the mean) in each partition.
- Instead of using *entropy* we use *variance* to split: the **bigger the spread** in the set of output labels, the **less certain** we are about what the value of the leaf node should be
- The best split results in a large **reduction of average variance** over the created segments.

$$I_{S}(V) = \text{Var} (S) - \sum\limits_{i} \frac{\lvert S_{i} \rvert}{\lvert S \rvert} \text{Var}(S_{i})$$
## Handling Numeric Features
Numeric features can be handled by determining optimal split point. For example, a feature $x$ can be split at a threshold $t$, creating two branches: one for $x \leq t$ and another for ($x > t$) .

> [!tip] **Splitting Criteria for Regression Trees**
> - **Mean Squared Error (MSE)** Minimize the variance within each partition.
> - **Mean Absolute Error (MAE)** Minimize the absolute deviations within each partition.

- tree models are a model class that provide a **generalization hierarchy**: they can be both
	- **constant models** which output just one value for all instances (high-bias, low variance, underfitting), or
	- *full-depth* tree models, which are very likely to memorise irrelevant details of the data (low-bias, high variance, overfitting)

---
# 🤝 Ensembling
## Concept
Ensembling involves combining multiple models to improve overall performance. The idea is that a group of models, when combined, can compensate for each other's weakness.
- Reduces variance
## Techniques
1. **Bagging (Bootstrap Aggregating)**:
    - _Process_: Train multiple models on different bootstrap samples (random samples with replacement) of the dataset and aggregate their predictions (e.g., by averaging or voting).
    - _Example_: Random Forests, which are ensembles of decision trees trained on different subsets of the data.
2. **Stacking**:
    - _Process_: Combine the predictions of multiple base models using a meta-model, which learns to predict the target based on the base models' outputs.

> [!info] **Benefits of Ensembling**
> - _Reduced Overfitting_: Combining models can lead to more robust predictions.
> - _Improved Accuracy_: Aggregating diverse models often results in better performance than individual modes.

---
# 🚀 Boosting
## Concept
Boosting is an ensembling technique that sequentially trains models, each attempting to correct the errors of its predecessor. This approach focuses on difficult-to-predict instances, gradually improving the model's performance.
- Reduces bias

## Process
1. _Initialize_: Start with a base model that makes initial predictions.
2. **Iterative Training**:
- At each step, train a new model to predict the residual errors (differences between actual and predicted values) of the combined ensemble so are.
- Update the ensemble by adding the new model, often with a scaling factor (learning rate).

1. _Prediction_: The final prediction is the weighted sum of the predictions from all models in the ensemble.

## Example: Gradient Boosted Decision Trees (GBD)
GBDTs build decision trees sequentially, where each tree tries to correct the errors of the combined ensemble of previous tree. This method is popular for achieving high performance with relatively little effort.

> [!warning] **Considerations in Boosting**
> - _Overfitting_: Boosting can overfit if the number of iterations is too large.
> - _Computational Cost_: Sequential training can be time-consuming.

---
References: https://mlvu.github.io/lecture10/
---
Tags: 
Created: 2025-02-15 16:28:50
---
(Links:: [[Machine Learning]])
# Experiments
Models can be compared using [[Percom 8 Classification III#Classifiers evaluation|classifier evaluation]]. The easiest way to compare classifiers with by comparing their *errors*.
> [!warning] Never judge your performance on the training data
> Due to *overfitting*, a classifier might be perceived as being better

- The larger the test set, the more precise our estimate of our model's error
- **Do not test multiple times**

> [!info] Evaluation Recipe
> 1. Split your data into *train* and *test* data
> 2. Choose your model, hyperparameters, etc. only using the *training* set
> 3. State your hypothesis (ex. kNN with $k=7$ beast existing model $X$)
> 4. Test your hypothesis **once** on the *test* data

> [!info] Validation set
> `| Training | Validation | Test |`
> - During model and hyperparam. selection: train on *training* and test on *validation*
> - Final run: train on *training and validation* and test on *test* (should give the same error as with the validation set as a test)

- validation and test set should be the same size
- if you fear overfitting, you can rerun the experiment with a different train/validation split
- use training data (with validation withheld from training) to select hyperparameters for each dataset individually
- **cross-validation**: 
	- split data into 5 chunks
	- each hyperparameter does 5 runs (with the different splits of training data)
	- *average* of the scores is the final score
- be careful of temporal ordering of instances (if the time of the data plays a role)
	- in this case split data increasingly in order of time

> [!example]-
> ```
> |     training      |
> ---------------------
> |val|   |   |   |   | 0.3
> |   |val|   |   |   | 0.4
> |   |   |val|   |   | 0.1
> |   |   |   |val|   | 0.3
> |   |   |   |   |val| 0.4
> ```
> $$avg= \frac{1}{5}\sum\limits_{i}x_{i}=0.3$$

# Statistical Testing
- The **true metric** (what you measure) is unknown, however we can take a *random sample* to get an *estimate* of the true metric
- **true accuracy**: probability a classifier correctly classifies an instance
	- We can never find this value but only *estimate* via a sample of the distribution
- **statistical testing**: how likely is it that the samples correlate with the true distribution (and as such that a classifier is better than another given some test on a sample)
	- this can be depicted using a [[Binomialverteilung|Binomial distribution]]
	- compare two classifiers we would look at the [[confidence intervals]] (e.g. 95%)
	- No overlap between confidence intervals of two models indicates that there is a significant difference

![|500](https://mlvu.github.io/evaluation/21.Methodology1.key-stage-0035.svg)

- *spread*: how much the randomness in our process (data sampling or search algorithm) affects the result
	- sampling a new dataset is easy with cross validation but not perfect
	- bootstrapping: resample data *with replacement*
# Metrics
- **Bias**: Difference between optimal MSE and true MSE
	- Occurs when model doesn't fit the "shape" of the data (underfitting)
- **Variance**: Difference between true MSE and measured MSE (changes when resampling data)
	- Occurs when model doesn't follow the natural flow of the data (overfitting)

![|500](https://mlvu.github.io/evaluation/21.Methodology1.key-stage-0056.svg)
- bias and variance are often a *tradeoff*
- **ensembling**: combine different models for variance *and* bias reduction
- **Class Imbalance**: The training data may be unequal, thus indicating if the classification error is good or not
	- Given a dataset with 1% positive instances, a model trained on this data with a classification error of 0.05 is bad (classifying everything as negative would yield better results)
	- Dataset may contain little of what is trying to be predicted
- **Cost imbalance**: How much *worse* is a mislabeled positive example than a mislabeled negative example?
- *Majority class classifier*: Used as a **base line** (assigns anything the class with most instances)
- [[Confusion Matrix|Confusion Matrix]]
$$
\begin{array}{c|ccc}
 & \text{pos} & \text{neg} \\
\hline
\text{pos} & 6 & 1 & 7\\
\text{neg} & 2 & 5 & 7\\
& 8 & 6
\end{array}
$$

[[Confusion Matrix|Precision and recall]] are two metrics that express a tradeoff between the two types of mistakes.  
- **Precision**: what proportion of the returned positives are actually positive?
- **Recall**: what proportion of the existing positives did we find?

![|500](https://mlvu.github.io/evaluation/21.Methodology1.key-stage-0078.svg)
- calculating accuracy on the *test data*, and comparing it to the *validation accuracy*, will show if the model is overfitting
# PR, ROC and AUC
Two other popular metrics are the **True positive rate** and the **False positive rate**.
An **ROC** curve plots these two metrics, as they commonly are a tradeoff:
![[ROC curves.svg|500]]
- a *timid* classifier would classify everything as false (bottom left)
- a *brave* classifier would be more daring and classify more things as correct (upper right)
- **Ranking Classifier**: in addition provides *score* of how negative or positive a point is 
	- moving the threshold dictates how eager it is

![|500](https://mlvu.github.io/evaluation/21.Methodology1.key-stage-0089.svg)

- **Ranking Error**: A pair of labels ranked the wrong way around
	- Ex: $t$ and $f$ form a ranking error ($t$ is more negative than $f$ even though it's positive and $f$ is negative)
- **Coverage Matrix**: 
	- negative points on horizontal axis; positive points on vertical axis
	- points predicted to be more positive are further left and down

![|500](https://mlvu.github.io/evaluation/21.Methodology1.key-stage-0091.svg)
- We can create a new classifier between any pair of classifiers in the ROC, and choosing a probability, say of 50/50, to end up in the middle of a line between them
- **Area under the curve (AUC) is an estimate of the probability that a ranking classifier puts a randomly drawn pair of positive and negative examples in the correct order.**
- Normalizing the coverage matrix gives us the ROC space

> [!example]- Ranking Decision Tree
> ![|500](https://mlvu.github.io/evaluation/21.Methodology1.key-stage-0099.svg)
> - points are grouped into areas
> - since there is not *distance* to a regression function, they are ranked based on *proportion of positive points* in the area
> - pairs ranked as "the same" are colored orange

- *PR curve* plots precision against the recall

> [!summary] Important Points
> - The confusion matrix is used to gauge a *single classifier*
> - AUC is a metric for a *collection of classifiers*, usually derived from a *ranking classifier*
> - How to turn a classifier into a ranking classifier depends on the type of classifier
> 	- For linear classifiers, take the distance to the decision boundary
> 	- For tree classifiers, sort by class proportion in each segment
> - AUC is a good metric if we don't know the relative importance of the classes, or if the classes are unbalanced
# Social Impact 2
- Consider history: Be aware of mistakes made in the past, so that we can be careful not to repeat them
- Are you looking at what you think you're looking at?
	- Other features might give contribute to the prediction although not intended
	- The performance we see doesn't mean we're seeing it for the *reasons* we were hoping for
- Are you predicting what you think you're predicting?
	- What we are predicting is likely a combination of factors that have a correlation to that label
	- Facial expressions might contribute to the labeling, when we try to predict based on facial features
- What *different* hypotheses explain the observed effect?
- What do positive results *mean*?
	- Ex: Height and gender have a slight correlation that allows us to make an educated guess for certain parts of the range of heights

- How do you frame your research?
	- Consider which features you are using
	- Consider multiple hypotheses; Social, biological, personal
		- Train yourself to always come up with different explanations for a given set of facts
	- Distinguish between *detecting*, *predicting* and *guessing*
		- Even 0.91 AUC is more guessing than predicting. It's only detecting if you *strictly control your features*
# No Free Lunch
> ... Any two **optimization algorithms** are equivalent when their performance is average across all possible problems
> ~ [Wolpert & MacReady 1997](https://en.wikipedia.org/wiki/No_free_lunch_theorem)

> [!example]-
> Given some data $X$ and basic methods $A$ and $B$.
> Meta-methods:
> - **method $C$**: Use a data split, choose whichever performs the best.
> - **method $D$**: Use a data split, choose whichever performs the **worst**.
> 
> According to the NFL theorem, there are as many datasets $X$ for which $C$ beats $D$ as there are for which $D$ beats $C$

- Just from looking at the data we don't know which learning method will perform the best
- Not all datasets are *equally likely* (NFL assumes the opposite)
	- There is some non-uniform distribution that tells us which datasets are more likely than others -> we can work out a universally best learning algorithm
- **simple data** is more likely than complex data -> we should have a simplicity bias

> [!attention] The no-free-lunch *principle*
> There is no single best learning method. Whether an algorithm is good, depends on the domain.

---
References:
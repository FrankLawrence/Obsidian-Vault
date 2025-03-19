---
Tags: 
Created: 2025-02-27 11:57:18
---
(Links:: [[Machine Learning]])
# Missing values and outliers
- **do not take your data at face value** (eg. survivorship bias)
- Metrics can be the same, even thought the data looks very different
	- in the below example, all datasets have the same mean and variance for $x$ and $y$
	- certain **patterns** might not be visible via metrics

![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0005.svg)
- higher dimensional datasets can be visualized with a **scatter plot matrix** (scatter plot between any two features)
- cleaning data involves: 
	- fixing missing data (including labels and values)
	- removing outliers
	- dealing with Class Imbalance

> [!question]- What to do with missing data (in feature or target column)?
> 1. Remove the feature entirely
> 2. Remove only that instance -> might transform data distribution
> 	- plotting a histogram of different features can tell if data is missing uniformly
> 3. Find out why data is missing

- if we can expect missing data **in production** then we may want to introduce missing values *artificially* in our test data
	- report the uncertainty, from the test set, created by missing values
		- Classification: the best possible and worst possible accuracy, depending on if all missing values were predicted correctly or all falsely
- if production does have missing data, have none in the validation set so that you can *test what method* of dealing with missing training values works best
- **Imputation**: fill in missing values
	- categorical data: **mode**
	- numerical data: **mean** or **median**
- we can also *predict the missing values*
- *outliers* are catogorized into two type: natural (something that appears in the data but usually doesn't match the distribution naturally) and unnatural (could be something corrupt)

> [!summary]- Outliers
> - Are they mistakes?
> 	- Yes: Deal with them
> 	- No: Leave them be; check your model for strong assumptions of normality
> - Can we expect them in production?
> 	- Yes: Make use the model can deal with them
> 	- No: Remove. Get a *test set* that represents the production situation

- *squared errors* assumes normality -> bad on non-normal distributions (eg. with natural outliers)

> [!note]- Optimum of squared errors is mean
> If we want to impute a value, we can try finding a good value by minimizing squared errors of a single value
> $$\begin{align}
> \underset{m}{\text{arg min}}\sum\limits_{i}(m-x_{i})^{2}\\
> \sum\limits_{i}\frac{\partial(m-x_{i})^{2}}{\partial m}&=0 \\
> \sum\limits_{i}\frac{\partial(m-x_{i})^{2}}{\partial m-x_{i}}\frac{\partial m-x_{i}}{\partial m}&=0 \\
> 2\sum\limits_{i}m-x_{i}&=0 \\
> nm-\sum\limits_{i}x_{i}&=0 \\
> m &= \frac{\sum_{i}x_{i}}{n}
> \end{align}$$
> This shows that we can use the mean as a representative example

- using *mean absolute error* leads to outliers not having such drastic of an impact

> [!note]- Optimum of absolute errors is median
> This time we would like to word out a representative value of a set where outliers are present
> $$\begin{align}
> \underset{m}{\text{arg min}}\sum\limits_{i}\vert m-x_{i}\vert\\
> \sum\limits_{i}\frac{\partial\vert m-x_{i}\vert}{\partial m}&=0 \\
> \sum\limits_{i}\frac{\partial\vert m-x_{i}\vert}{\partial m-x_{i}}\frac{\partial m-x_{i}}{\partial m}&=0 \quad (\text{the absolute function $|x|$ has derivative $1$ for $x>0$ and derivative $-1$ for $x<0$}) \\
> \sum\limits_{i}\text{sign}(m-x_{i})&=0 \quad (\text{sign returns $1$ for positive values and $-1$ for negative values})\\
> \end{align}$$
> This shows that we can use the median as a representative example

# Class Imbalance and feature design
- we could be trying to implement a classifier, however within the data, only a fraction might be that which we are trying to identify
- **oversample**: add copies of randomly sampled instances of *minority class*
	- SMOTE: finds clusters in minority and generates means as new points
- **undersample**: remove randomly selected instances of majority class
- over- and undersample change data distribution

Some algorithms only work with numeric or categorical features, or even both.
- integer valued to category -> group into bins (eg. below and above median)
- extract potential useful information (eg. phone numbers have area codes)
- categoric to numeric
	- simply using *integer coding* would impart a *false ordering*
	- *one-hot coding*: turns one categorical feature into several numeric features

Linear classifiers are limited in the relations they can represent. The below image depicts the *XOR problem*
![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0042.png)
- **adding extra features derived from the existing features** can help the linear classifier
- using the *cross-product* of $d$ and $p$ allows a linear classifier to simply look at the sign

$$\begin{array}{c|c|c}
d & p & d \times p \\ \hline
{\color{#58F}0.75} & {\color{#58F}0.98} & {\color{#58F}0.74} \\
{\color{#F05}-0.66} & {\color{#F05}-0.32} & {\color{#58F}0.21} \\
{\color{#F05}-0.45} & {\color{#58F}0.84} & {\color{#F05}-0.38} \\
{\color{#58F}0.93} & {\color{#58F}0.78} & {\color{#58F}0.72} \\
{\color{#F05}-0.42} & {\color{#58F}0.24} & {\color{#F05}-0.10} \\
{\color{#F05}-0.02} & {\color{#58F}0.43} & {\color{#F05}-0.01} \\
{\color{#F05}-0.74} & {\color{#58F}0.58} & {\color{#F05}-0.43} \\
\vdots & \vdots & \vdots 
\end{array}$$
- for circular models we can use the *cross product with itself* (for features $x_{1}^{}$ and $x_{2}^{}$ we use $x_{1}^{2}$ and $x_{2}^{2}$ instead) to create a model that is linearly separable 
![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0045anim1.svg)
- In the above example we classify as red if $x_{1}^{2}+x_{2}^{2}<0.7^{2}$ otherwise as blue.
- by adding second feature ($x^{2}$) derived from the first, we can create a simple but powerful linear model
	- if we don't have an intuition of useful features, **add all cross products** of all features with each other and themselves

![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0051.svg)

> [!summary] Adding features
> - Can make weak classifiers (especially a linear one) stronger
> - Any function of one or more of the existing features can work
> - The problem stays *convex*: easy to solve, optimal solution guaranteed
> - Common choice: all 2-way cross products, all 3-way cross products (there's a lot, but cheap to fit for linear classifiers)

  
# Normalization
- some features might not have same minimum or maximum -> bad for [[Introduction to Machine Learning#Classification|kNN classifiers]] -> must be normalized


- **Normalization**: for each feature, scale data linearly so that $0 \leq z \leq 1$ $$z= \frac{x-x_\text{min}}{x_\text{max}-x_\text{min}}$$
- **Standardization**: Rescale data so that $\mu$ becomes zero and $\sigma$ becomes 1 -> appears to come from a [[Standard normal distribution]] $$z=\frac{x-\mu}{\sigma}$$ For this we estimate the mean and standard deviation so that we can transform it back into a standard normal distribution (works if the data is roughly normally distributed)
	- the data is not normalized correctly if we normalize *correlated* features, since we standardize each feature independently
- Instead of transforming the data, we transform our coordinate system via the [[Linearly Independent Sets; Bases|Bases vectors]]
- **Whitening**: Find a set of new *basis vectors* so that we can express the data in a coordinate system where the features are not correlated
	- standard MVN: mean at origin and identity matrix as its covariance matrix $$\mu=\begin{pmatrix}0\\0\end{pmatrix}\quad \Sigma = \begin{pmatrix}1 & 0 \\ 0 & 1\end{pmatrix}$$
	- Maximum likelihood estimators for **sample mean $m$** and **sample covariance $S$** $$\begin{align}m &= \frac{1}{n}\sum\limits_{i}x_{i} \\ X &= [x_{1},...,x_{n}]-m \\ S &= \frac{1}{n-1}XX^{\top} \end{align}$$

![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0074anim2.png)

# Principal Component Analysis
- **Feature Selection**: Find subset of features that is most informative -> retain meaning of features
- **Dimensionality reduction**: Map the features to a new smaller set of features; Retain as much information as possible. 
	- Reasons to reduce dimensionality: *Efficiency*, *Reduce Variance*, *Visualization*
	- correlating features can be removed (one is redundant)
## Linear Reduction
- $c'$ determines the direction of a line that approximates the feature $x$
![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0083anim5.svg)
- for a single feature we use a single scalar value representing the entire instance $x$: $z$
- *reduction function*: dot product of our vector with some parameter vector $c'$
- *reconstruction function*: multiply reduced representation with some other parameter vector $c$

> [!question]- Why is the optimal solution that $c'=c$?
> - the best value of $x'$ on a predefined line given by $c$ is a value $z$ such that $x'$ is an  [[Orthogonal Projections|orthogonal projection]] of $x$ onto $c$. ![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0084anim1.svg)
> $$\begin{align}
> ||zc|| &= ||x|| \cos \alpha \quad (\text{from basic trigonometry})\\ 
> &= ||c|| \cdot ||x|| \cos \alpha \quad (\text{we assume that $c$ is a unit vector}) \\
> &= x^{\top}c 
> \end{align}$$
> - the orthogonal projection of $x$ onto $c$ is the value we want to multiply $c$ by -> the best vector $c'$ that would be closest to $x'$ is $c$

- optimizing $c$ is a lot like [[Linear models and search#Linear Regression|linear regression]], where we try to choose $c$ such that the squared distance between data and reconstruction is minimal:
  $$\begin{align}
  \underset{{\color{orange}c}}{\text{arg min}} &\sum\limits_{{\color{#58F}x}} ||{\color{#58F}x'}-{\color{#58F}x}||^{2} \\
  = \underset{{\color{orange}c}}{\text{arg min}} &\sum\limits_{{\color{#58F}x}} ||z\cdot{\color{orange}c}-{\color{#58F}x}||^{2} \\
  = \underset{{\color{orange}c}}{\text{arg min}} &\sum\limits_{{\color{#58F}x}} ||{\color{#58F}x}^{\top}{\color{orange}c}\cdot{\color{orange}c}-{\color{#58F}x}||^{2} \\
  = \underset{{\color{orange}c}}{\text{arg min}} &\sum\limits_{{\color{#58F}x}} \sqrt{\sum_{i}({\color{#58F}x}^{\top}{\color{orange}c}\cdot{\color{orange}c}_{i}-{\color{#58F}x})^{2}}^{2} \\
  = \underset{{\color{orange}c}}{\text{arg min}} &\sum\limits_{{\color{#58F}x},i} ({\color{#58F}x}^{\top}{\color{orange}c}\cdot{\color{orange}c}_{i}-{\color{#58F}x})^{2} \qquad \text{such that }||{\color{orange}c}||=1\\
  \end{align}$$
- The resulting vector $c$ is called the **first principal component**

We can reduce to more than one dimensionality: keep first principal component fixed, the second is orthogonal to the first such that it minimizes the reconstruction loss
- each next principal component is orthogonal to all previous ones
- first principle component is direction in which the *variance* is maximal
- second principle component is direction, orthogonal to first, in which the remainder of the variance is maximal

Choosing the number of dimensions to reduce (number of components) can be modelled as different hyperparameters. Plotting variance/reconstruction loss against number of components shows an *inflection point* 

> [!note]- Number of components = Number of features
> - gives perfect reconstruction
> - expresses data in another basis: **PCA basis**
> 	- the same as a [[#Normalization|whitening]] of the data ![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0096.svg)
> 	- principal components $c$ are unit vectors and are all orthogonal to each other -> forms an orthonormal basis
> 	- **PCA whitening**: multiply components with standard deviation of data projected onto $c$ gives whitening basis

- PCA for dimensionality reduction gives whitened data (if we standardize afterwards)

> [!summary] PCA
> - linear transformation that minimizes reconstruction loss
> - or: linear transformation that maximizes variance
> - also: whitening transformation
> - orders dimensions by variance captured/rec. loss (The first dimension is the most important for reconstructing the data, then the second and so on)

> [!example]- What's the point?
> ![|500](https://mlvu.github.io/preprocessing/22.Methodology2.key-stage-0103anim1.png)
> Here is an example of what PCA can tell us about a high-dimensional dataset.  
> In this research, the authors took a database of 1387 Europeans and extracted features from their DNA. They used about half a million sites on the DNA sequence where DNA varies among humans (i.e. 1387 instances: people, and 500k features: DNA markers). They also recorded where their subjects (or their immediate ancestors) were from.  
> 
> Only the DNA data was fed to the PCA algorithm, with the person’s origin only used afterward to color the points.  
> 
> It turns out that the two principal components of this data largely express how far north the person lives, and how far east the person lives. This means that if you plot the data in the first two principal components, **you get a fuzzy picture of Europe**.  
> 
> In short, the large scale geography of Europe can be extracted from our DNA. If I sent a large sample of European DNA to some aliens on the other side of the galaxy who’d never seen our planet, they could use it to get a rough idea of our geography.

- because some information goes lost when using PCA, we throw away noise that the model would otherwise *overfit* on -> can lead towards high bias

---
References: https://peterbloem.nl/blog/pca
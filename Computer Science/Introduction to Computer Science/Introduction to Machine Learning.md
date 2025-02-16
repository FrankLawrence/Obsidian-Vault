---
Tags: 
Created: 2025-02-06 23:00:49
---
(Links:: [[Machine Learning]])
# Introduction to ML
Machine learning is the area of computer science that relates to the capability of computers to **learn from data**.
Three types of machine learning tasks
- **Supervised learning** is the task of inferring a classification or regression from labeled training data.
- **Unsupervised learning** is the task used to draw inferences from datasets consisting of input data without labeled responses.
- **Reinforcement learning (RL)** is the task concerned with how software agents ought to take actions in an environment in order to maximize cumulative (delayed) rewards.
## Deep Learning
- Update/transform parameters of network with errors of previous tests.
- Input activates other neurons that are triggered on different parameters.
- All the parameters of a neural network are trained in order to **minimize a cost function**.

# What is Machine Learning
![Envelope|500](https://rafalab.dfci.harvard.edu/dsbook/ml/img/how-to-write-a-address-on-an-envelope-how-to-write-the-address-on-an-envelope-write-address-on-envelope-india-finishedenvelope-x69070.png)

Take a look at the above image. We humans can easily tell that the postcode is 02138. How do we know this? From looking at (and learning from) *a lot* of examples of numbers over the years. The same idea is applied to machine learning.

> [!question] What makes a suitable machine learning problem?
> - We *can't solve it explicitly*
> - Approximate solutions are fine
> - Limited reliability, predictability, interpretability is fine
> - Plenty of examples to learn from
> 
> | Bad                | Good                       |
> | ------------------ | -------------------------- |
> | Computing taxes    | Recommending a movie       |
> | Clinical decisions | Clinical decisions support |
> | Parole decision    | Predicting driving time    |
> | Unlocking phone    | Recognising a user         |

> [!definition] 
> Machine learning provides *systems* the ability to automatically learn and improve *from experience* without being *explicitly programmed*.

Systems that keep on learning on previous experience are called **learning agents**. We identify various subfields of machine learning: 
- [[Reinforcement Learning]]: We define the agent, environment and rewards system. Actions are taken to maximize rewards, which in turn change the environment.
- **Online learning**: The agent makes predictions, which we use as training material for future predictions.
- **Offline learning**: We *separate* learning from acting; thus programs that have already completed training, will never learn while they run.

To be able to create such agents, we need a *framework* such that we can make them specific to all sorts of applications. To make this possible, machine learning uses **abstract tasks** (e.g. classification, regression or clustering). Such abstract tasks can fall under two categories:

| Supervised                                      | Unsupervised                                            |
| ----------------------------------------------- | ------------------------------------------------------- |
| Explicit examples of input and *output*         | Only inputs provided                                    |
| Learn to predict the output for an unseen input | Find any pattern that explains something about the data |

**Supervised learning tasks**:
- [[#Classification]]: Assign a "class" to each example
- *Regression*: Assign a number to each example

Machine learning is often misinterpreted: 
- *Artificial Intelligence*: An encompassing field, that also deals with other non-machine learning associated tasks (e.g. Automated reasoning, planning)
- *Data Science*: An encompassing field that does always use machine learning (e.g. Gathering data, harmonising data, interpreting data)
- *Data Mining*: ML is used to create software (a model) from data, whilst DM analyses data to produce information *about* the data
- *Information Retrieval*: IR is a type of classification task (documents are classified into relevant and irrelevant)
- *Statistics*: Both analyze and model data in some way; methods of statistics aim to get at the *truth*, whilst ML tries to come up with something that *works*.
- [[Deep learning]]: A subfield of ML
# Classification
- We provide examples (*instances*) and we try to assign it to a class (*target value*) based on the *features* that we choose
- A learning algorithm is given the data and produces a *classifier*
	- The classifier takes new data in, and makes a guess what the correct class is

> [!example] [[OCR]]
> We can many images of a numbers, and flatten it into an array of digits, each representing the brightness value of a pixel. The target is one of ten classes: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
> Machine learning can learn very easily on this, and produces great results (the best classifier of OCR is 99,79% correct), however there is still a lot of work required to turn this solution into a working production system.

- Some problems are not as simple as OCR, and require more advanced features (e.g. chess)
	- **Domain expertise**: Can translate different states into good features, ex:
		- are there passed pawns
		- rooks on an open file
		- does a player own both bishops

- After abstracting problems into abstract tasks, we can use a **classification algorithm**. Here are some poplar types: 
	- Linear classifier
	- Decision Tree classifier
	- Nearest neighbours classifier
- We can plot the input data into a **feature space**. Here is an example with penguins ![Relation body mass and flipper length to sex|500](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0035.svg)
	- For each feature we would add another dimension
	- We can draw a line through the graph, where on one side a data point will be assigned to "female" and on the other to "male"
	- The line in 2D can be described as $ah+bd+c=0$. One multiplier for each feature ($a$ and $b$) and a value that is added independent of the features ($c$)
	- All lines in 2D are part of a 3D **model space**
	- We find the *best* possible model through a **loss function**
- **Loss function**: $$\text{loss}_\text{data}(\text{model}) = \text{performance of model on the data}$$ 
	- it tells us how much we like a given model for the current data (the lower the better)
	- it has the *model* as it's argument and the *data* as a constant
- In some cases using a single line doesn't cut it
- **Decision tree**: each feature is a node, where we move left if the feature is lower than some threshold value, and right otherwise:
  ```mermaid
  graph TD
    A((f > 1.9?))
    B((b > 4.5?))
    C((b > 5.3?))
    A -->|yes| B
    A -->|no| C
    D[male]
    E[female]
    F[male]
    G[female]
	B -->|yes| D
	C -->|yes| F
	B -->|no| E
	C -->|no| G
  ```
	- The shape the classifier draws in feature space to segment two classes is called the **decision boundary**: ![|500](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0043.svg)
- An alternative is **$k$-Nearest neighbours**, where there is no learning. A new point simply chooses the most frequent class out of the $k$ nearest neighbours and chooses that
	- $k$ is a **hyperparameter**
	  ![|500](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0045.svg)

The data classified is often not only classified into two types of classes, here are a couple variations:
- **Binary classification**: Two classes
- **Multiclass classification**: More than two classes
- **Multilabel classification**: None, some or all classes may be true
- **Class probabilities/scores**: The classifier reports a probability or score for each class

> [!abstract] The basic recipe
> 1. Abstract (part of) your problem to a **standard task**: Classification, Regression, Clustering, Density estimation, generative modelling
> 2. Choose your **instances** and their **features**: For supervised learning, choose a target
> 3. Choose your **model class**: Linear models, decision trees, kNN
> 4. **Search** for a good model: Usually a model comes with its own search methods. Sometimes multiple options are available

# Other abstract tasks
## Regression
This is another type of supervised learning, but instead we are predicting a *number* in $\Bbb{R}$. This can be modelled as such
- $x_{i}$: Features of an instance $i$
- $t_{i}$: True label of instance $i$
- $f(x_{i})$: The model is a function $f$ which should return a value close to $t_{i}$

To determine how good a model is, we can use the following loss function (aka **mean-squared-error** loss): $$loss(f) = \frac{1}{n} \sum_{i}\big({\color{red}f(x_{i})-t_{i}}\big)^{2}$$
The **residual** (marked in red) is the difference between the predicted value and the actual data (this should be small). Here are the predictions depending on the chosen model:

![](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0056.svg)
![](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0057.svg)
![](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0058.svg)

## Unsupervised tasks
### Clustering
Here the model has to group the instances via a structure that it must deduce from the data. The model is *not* given any sort of classes, but instead only *how many* different classes we expect. The algorithm **k-means** is very popular and works as follows:
- Take 3 random points in the feature space (called the "means") and assign each point in the space to the mean closest to them
  ![|500](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0063.svg)
- Re-compute the means to be the mean of all the points that belong to that cluster
  ![|500](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0064.svg)
- Repeat until it settles. The clusters are astonishingly close to the actual real world data ![|500](https://mlvu.github.io/introduction/11.Introduction.0.key-stage-0067.svg)
### Density Estimation
Here we want to learn how *likely* new data is. Each prediction of an instances gives how likely the model thinks the given instance is.
- the same as modelling a *probability distribution*
### Generative Modelling
- some data is easier to *sample* than to get a probability (density) estimate
- generative modelling is the process of creating models which give new examples
## Self-supervised learning
- Labeled data is often expensive
	- We can train models both on labeled and unlabelled data
- **self-training**: 
	- classifier trains on labeled data
	- labels the unlabelled data itself
	- trains again on the entire data set
- **self-supervised learning**: large unlabelled data set can train a model with little or no annotation required

![[Abstract tasks of Machine Learning.canvas|Abstract tasks of Machine Learning]]
> [!summary]
> - **abstracting** your problem: *instances*, *features*, *target*
> - **supervised** learning: classification, regression
> 	- Linear models, tree models, NN models
> - **unsupervised** learning: clustering, density estimation, generative modeling
> - **loss function**: maps a choice of model to a *loss* for the current data (the lower the better)

# Social Impact
> Given that a person's gender cannot be inferred by appearance, we have decided to remove these labels in order to align with the artificial intelligence principles at Google, specifically Principle #2: avoid creating or reinforcing unfair bias. After today, a non-gendered label such as 'person' will be returned by Cloud Vision API.
- often discussed sensitive topics include
	- sexual orientation
	- race, ethnic identity, cultural identity
	- gender and/or sex
- What makes an attribute sensitive?
	- can it be used for *harm*?
	- can mischaracterisation be *offensive*?
	- is it commonly used to *discriminate*?
- some models can unintentionally be biased by race, without any race being specified in the data
	- COMPAS a tool used for profiling criminals was more likely to deny black people due to **inferring** this feature from postcodes
- when gathering data, not all variables are considered carefully which can lead to **bias** -> models *inherit* the bias of the data gathered
	- white people might be overrepresented in the data set leading to it not working well on non-white people
- another challenge is deciding on the correct *action* even if the prediction is correct
	- a translator model can predict that gender neutral term is translated to be male with 70% accuracy, however the user might not always want that
- Accuracy isn't the only consideration for predictions
	- Relevant features with a causal relation are more important than correlational ones
	- Informative features can provide better predictions for rare or critical events
- Predicting certain attributes can be offensive or hurtful, regardless of accuracy
	- Consider whether predictions may cause harm, especially when applied to millions of people
	- Avoid *guessing* sensitive attributes, and instead follow social norms and treat them with respect
- **Correlation** does not imply **causation** in machine learning predictions
	- Offline machine learning *cannot* determine causal relationships
	- Publishing correlation results can imply a causal link, even if not intended

> [!question] Should we include sensitive attributes in data at all?
> - We need certain attributes to study bias
> - Removing attributes does not always lead to clean data
> - We can include sensitive attributes with consent

- Using sensitive attributes as targets for classification is *not* a straightforward decision
	- Some algorithms can learn representations that predict sensitive attributes, even without explicit targets
	- Building a system that predicts sensitive attributes can be used to *warn* people of potential dangers, but it can also cause harm
- Predicting sex or gender can be problematic, especially for transgender people who may experience mental health issues due to gender dysphoria
	- A significant proportion of people (0.6%) identify as transgender, and this number may be higher due to stigma
	- Predicting sex or gender can cause harm, intentionally or not, especially in countries where non-conformity to gender norms is illegal
	- However, predicting sex or gender can also be used for beneficial purposes, such as in medical applications where sex is a relevant factor
- It's essential to consider the potential harm and benefits of predicting sensitive attributes and to be careful when using features that may imply causal links
- Classification can be a **shallow abstraction** of complex phenomena, and it's essential to acknowledge this when working with sensitive attributes
- The categorizations used in classification can be inadequate for real-world applications, and it's essential to consider the limitations of these models
- It's crucial to listen to users and consider the evidence when working with sensitive attributes, and to be mindful of the potential impact of machine learning systems on individuals and society
# Generalization
- We don't want our model to most accurately predict the *training data*, instead we want it to be general such that it can make reasonably good predictions on *unseen data*
	- *outliers* in our training data can lead to very bad results when (aka **overfitting**)
	- in such cases we say the model is **memorizing** data
- we *withhold data*, and use it to test *after* picking a model
- The task is to fit the **pattern** in the training data and discard the **noise**

## The problem of induction
- **Inductive reasoning** is a type of learning where we observe something happening multiple times and *infer that it will probably happen again*, but it's not logically certain
	- similar to finish-the-sequence puzzles, where we infer the rules and apply them to find the missing element
- Inductive reasoning is different from **deductive reasoning**, which is rule-following and what computers do best
- inductive reasoning cannot be proved to work by deductive methods, making it a challenging problem to reduce to rules
	- The problem of induction is still unsolved; while we can teach computers to learn, we don't fully understand the rules
- There is no general theory of learning that is always obviously correct, and solutions to machine learning problems often rely on *intuition*
	- preferring simple solutions over complex ones seems to give good results, but is not always the case (e.g. linear regression example was too simple)

---
References: https://mlvu.github.io/introduction/
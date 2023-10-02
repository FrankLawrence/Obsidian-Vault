---
Tags: 
Created: 2023-09-29 12:02:26
Links: "[[Pervasive Computing]]"
---
# Probabilistic Classifiers
## Probability basics
**Uncertainty is everywhere**:
- Sensors are often subject to errors. The stored data is only an older, approximated version of the entity being monitored.
- We can only say :"*the probability* that the car is situated at 4m from the access point *is 0.9*", "the room temperature is in the interval 10° - 20° with the probability of 0.7".
- This is probabilistic language

Probabilistic learning:
- Probability models view learning as a process of reducing uncertainty.
- Used a lot in modern pattern recognition.

Discrete Random Variables:
- A is a Boolean-valued *random* variable if A denotes an event, and there is some degree of uncertainty as to whether A occurs

Probability $P(A)$
- We write $P(A)$ as "the fraction of possible worlds in which $A$ is true"
- $P(A)$ is an *unconditional probability*
- $P(A|B)$ (*conditional probability*) is the probability that $A$ is true given that $B$ is all we know $$P(A|B)=\frac{P(A \text{ and } B)}{P(B)}$$
- Bayes rule: $$P(B|A)=\frac{P(A|B)\times P(B)}{P(A)}$$
	- The essence of the Bayesian approach is to provide a mathematical rule explaining how should our existing beliefs $P(B)$ change in the light of new evidence $A$.
## Naïve Bayes classifier
- Given a set of classes: $C_1, C_2, ...C_i,..C_n$ with prior probabilities $P(C_1), P(C_2), ...$
- Use Bayes rule to calculate the posterior probabilities that an object with the features vector $X$ belongs to class $C_i, P(C_i|X), i=1..n$
- The class with the highest posterior probability is the most likely class.
## Bayes networks
## Hidden Markov Models
# Unsupervised learning
- Do not have any training sets
- They explore data and search for naturally occurred patterns and clusters within it.
- Once the clusters have been found we can make decisions
# Reinforcement learning
# Classifiers evaluation

---
References:
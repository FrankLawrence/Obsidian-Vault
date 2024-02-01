# Theoretical exercises
## Exercise 1
Read Exercise 1.3 from the handout Additional exercises (available on canvas) and consider the same setting here.

1. What is the probability that a random person, which does the test, gets a positive result? Explain the difference between this probability and the probability you are asked to calculate in the Exercise 1.3 from the set of additional exercises.
2. Solve Exercise 1.3 from handout Additional exercises.
3. Are the two events that a person has cancer and that the test is positive dependent? Does the fact that a test result was positive increase the risk of having cancer, when compared to the probability that a random individual from the population has cancer?
## Exercise 2
Suppose you walk to a bus stop to catch a bus; busses of the line you take arrive there every 15 minutes. You don’t look at your watch and arrive at the bus stop totally at random. For simplicity, we assume that all arriving times (you/the busses) are rounded down to whole minutes. When you and the bus arrive simultaneously, you will be able to catch it.

1. Describe the probability space that models the above-described waiting time experiment, i.e. sample space and the related probabilities.
$ml:\text{Minute late}$
$\Omega=\{\text{On time},\text{1ml},\text{2ml},\text{3ml},\text{4ml},\text{5ml},\text{6ml},\text{7ml},\text{8ml},\text{9ml},\text{10ml},\text{11ml},\text{12ml},\text{13ml},\text{14ml}\}$
$P(\text{On time})=P(\text{1ml})=\cdots=\frac{1}{15}$

Suppose now that you still start walking towards the bus stop at a completely random time; but that you do look at your watch at the beginning of your journey. Knowing your walking pace and the distance to the bus stop, you know by how many minutes you will miss the previous bus. If you would miss the bus by 4 or less minutes (i.e. hypothetically, the waiting time would be 11 minutes or more), you decide to hurry up; in this case, you will still be able to catch it. Otherwise, if you would miss the bus by 5 or more minutes (i.e. waiting times 10 minutes or less), you simply go on with your normal walking pace.

1. Let the random variable $X$ model your waiting time at the bus station in the just-described situation. Calculate the probability that you will need to wait for at least 5 minutes.
   $X(\text{On time})=X(\text{1ml})=X(\text{2ml})=X(\text{3ml})=X(\text{4ml})=0$
   $X(\text{5ml})=5, \cdots, X(\text{14ml})=14$
   $$P(\text{Wait at least 5 minutes})=\sum\limits_{\omega:X(\omega)\geq5}P(\{\omega\})=\frac{10}{15}=\frac{2}{3}$$
2. Calculate the expectation of $X$.
   $$E(X)=\sum\limits_{i=1}^{|\Omega|}x_{i}\times P(X=x_{i})=0\times \frac{5}{15}+1\times 0\cdots +14\times \frac{1}{15}=7 \frac{1}{3}$$
3. Calculate the variance of $X$.
   $$\sigma^{2}=E(X-EX)^{2}=\sum\limits_{i=1}^{15}\left[x_{i}^{2}P(X=x_{i})\right]-\mu^{2}=5^{2}\times\frac{1}{15}+\cdots+14^{2}\times \frac{1}{15}-53 \frac{7}{9}= 11 \frac{2}{9}$$
4. Suppose you walk to the bus stop in the previously described way year 160 times per year. Denote by $X_{1},...,X_{160}$ your waiting times on all these days; assume they are independent of each other. Describe the (approximate) distribution of your average waiting time X ̄160 = 1 P160 Xi across the whole year.
   From the law of large numbers, we know that their mean $1/160(X_{1}+\cdots +X_160)$ tends to approach $\mu$

Note: only if you are not able to derive the expectation in c) you may continue in d) and e) with the wrong value E(X) = 5, and if you are not able to derive the variance in d), you may continue in e) with the wrong value var(X) = 4.
# R-exercises
Hints concerning R:

- Recall that a simple random sample of size n from a set of values x can be drawn in R using the function `sample(x,n)`. By default, the sample is drawn without replacement; by setting the additional parameter replace to TRUE, the sample is drawn with replacement. This function can be used to simulate a die.
- A sample from a certain distribution can be obtained in R with the function `rdist(n,par)` where `dist` stands for the name of the distribution, n for the sample size, and par for the relevant parameters: `x=rnorm(50,5,1)`, `x=rexp(25,1)`, `x=runif(30,-1,1)`, `x=rt(10,df=5)`, `x=rchisq(25,df=8)`. For example, the function `trnorm(n,mean,sd)` generates a sample of size n from the normal distribution with expectation mean and standard deviation $sd$. The parameters of the other distributions are documented in the help-function.
- A normal QQ plot can be obtained with `qqnorm(x)`, histograms with `hist(x)`,and box plots with `boxplot(x)`.
- The commands `dnorm(u)`, `pnorm(x)` and `qnorm(a)` compute respectively the values of the probability density function φ(u), the distribution function Φ(x) = P(Z ≤ x) and a-quantile for standard normal distribution. For non-standard normal distributions adjust the arguments of the function.
- The command `lines(x,y)` joins the corresponding points in the vectors x and y with line segments. This is useful to draw a curve on top of an existing plot. Similarly, `abline(a,b)` draws the line $a + bx$ on top of an existing plot. Otherwise specify `type="l"` in the parameters of the function `plot()`.
- To concatenate text and numbers (useful for titles of plots) use the R-function `paste()`.
- Use the command `set.seed(...)` to make your results based on the generated samples reproducible.
- In a normal QQ plot, data are compared to theoretical normal quantiles.
- Command `par(mfrow=c(l,k))` sets lk plots in l rows, with k plots in each.
## Exercise 3
1. Generate the following samples and make for each of them a normal QQ plot, a histogram and a boxplot. 
	1. one sample of size 115 from the `chisquared` distribution with degrees of freedom 2;
	2. one sample of size 105 from the t-distribution with 4 degrees of freedom.

Evaluate the usefulness of the normal distribution as a model distribution for both samples based on the QQ plots. Comment briefly on each plot and each peculiarity. Further, relate the peculiarities visible in the histograms to what you see in the corresponding box plots, and describe your findings. In particular, address the heaviness of the tails, symmetry, and outliers.

1. In this part you will practice with the normal distribution in R.
	1. For a standard normal distribution, by using R compute the following 3 probabilities: that an arbitrary outcome is smaller than 3, that it is bigger then -0.5 and that it is between -1 and 2.
	   ```r
		pnorm(3)           # P(X<=3)
		1-pnorm(-0.5)      # P(X>=-0.5) = 1- P(X<=-0.5)
		pnorm(2)-pnorm(-1) # P(X in [-1,2]) = P(X<=2) - P(X>=-1)
		```
	2. Repeat (i) for a normal distribution with mean 3 and variance 4. For this distribution, find also the value for which 95% of the outcomes is smaller. Now determine the same values by using the tables from the book and compare with those found by using R.
		  ```r 
		  pnorm(3,mean=3,sd=2)         # P(X<=3)
		  1-pnorm(-0.5,3,2)            # P(X>=-0.5) = 1- P(X<=-0.5)
		  pnorm(2,3,2)-pnorm(-1,3,2)   # P(X in [-1,2]) = P(X<=2) - P(X>=-1)
		  qnorm(0.95, 3,2)
		  ```
		From the book we can see that the $z$-score where 95% of the area is to the left is 1.645. The same value for a distribution with mean 3 and variance 4 is much larger, since the entire distribution is moved to the right and it is wider (larger sd).
	1. Any normal distribution can be derived from a standard normal distribution by multiplying with the standard deviation and adding the mean. Generate in this way a sample of size 1000 from a normal distribution with mean -1 and standard deviation 5 and verify that the sample mean and sample standard deviation are close to the theoretical values.
	   ```r
	   standard = rnorm(1000)
	   standard = standard*5-1
	   sd(standard)
	   mean(standard)
	   ```
	1. Generate two samples, one of size 100 and another of size 100000, from a standard normal distribution. Using these data, write a suitable R function to verify the results of (i), without assuming normality. Comment on your findings.
2. Answer for each of the data sets below the following question: “Is it reasonable to assume that the data come from a normal distribution?” In each case choose from the two answers: “Obviously not from a normal distribution” or “Normality cannot be excluded”. Base your answer on histograms, boxplots and normal QQ-plots. Also, for each dataset, point out the peculiarities of each sample by comparing the histogram, boxplot, and QQ-plot with each other. Indicate whether you detect (some/all) peculiarities in some/all of these diagnostic plots.
	1. diabetes.csv is the data file from a study about understanding the prevalence of obesity, diabetes, and other cardiovascular risk factors, to analyze the individuals’ total cholesterol values.
	   More information on http://biostat.mc.vanderbilt.edu/wiki/pub/Main/DataSets/diabetes.html Hints: use diabetes <- read.csv("diabetes.csv") to read the dataset.
	   Then use `diabetes$chol` to obtain the individuals’ total cholesterol values.
	2. vlbw.csv is the data file about newborn babies with very low birth weight, to analyze the baby birth weights.
	   More information on http://biostat.mc.vanderbilt.edu/wiki/pub/Main/DataSets/vlbw.html
	   Hints: use `vlbw <- read.csv("vlbw.csv")` to read the dataset. Then use `vlbw$bwt` to obtain the babies’ birth weights.

## Exercise 4
Study the R-function diffdice from the file function02.txt. Load it by using the command source("function02.txt").

1. Consider two dice and the random variable ‘the absolute difference of two die rolls’. Illustrate the Law of Large Numbers for this random variable by considering ‘the mean of the absolute difference of two die rolls’ in n trials for different values of n and making a plot similar to the one on a slide from Lecture 3.
   *Hint*: n trials means that both of two dice are rolled n times, and for each of the trials the absolute difference is calculated.
   You may use (without proof) that the theoretical expected value of the absolute difference is about 1.9444.

2. Use the function `diffdice` to find an approximate value of expectation of the random variable ‘the absolute difference of two die rolls’ and the probability of the event ‘the absolute difference of two die rolls is 3’.

3. Use the function `diffdice` to graphically illustrate the Central Limit Theorem for the random variable ‘the mean absolute difference of two dice rolls after n trials’ , by making 4 plots similar to the 4 plots on a slide from Lecture 4.
   You may use (without proof) that the standard deviation of the absolute difference of two dice rolls is approximately 1.4326.

4. Explain briefly why the 4 plots of part 3. illustrate the Central Limit Theorem in the present context.
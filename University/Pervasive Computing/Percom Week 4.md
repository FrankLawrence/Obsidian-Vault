---
Tags: 
Created: 2023-09-27 10:33:34
Links: "[[Pervasive Computing]]"
---
# Preparatory exercises
## PE1
Enumerate the steps in a classification process.
## PE2
What is a rule-based classifier? How does it work?  
## PE3
You have to answer either a) or b), depending on whether you chose to classify images or sounds.
1) Image: What does BLOB mean? Define the terms “form factor” and “Euler number”.

2) Sound: Define the concept of frequency spectrum. Sketch a sinusoidal signal of 5 Hz in time domain and its frequency spectrum and label the x-axis.
What is a spectrogram and how is this constructed?  
## PE4
What is an artificial neural network (ANN)? Draw an ANN for automatic character recognition where the images are binary 3x3 and we want to recognize only three letters A, B or C.  
## PE5
What is a confusion matrix? Draw a possible confusion matrix for the PE4 problem and describe it.

# Exercise 1
## A
Write a MATLAB script that can recognize objects from an image using a rule-based classifier. You don’t have to restrict yourself to the objects presented in this example; you could recognize other types of objects, such as keys, pens, etc. You can also recognize characters, based on geometrical features such as orientation, eccentricity and Euler number.
## B
Write a document in which you specify which classes you used, as well as which features and which rules you implemented in your classifier. Include the inputs you tested, and the corresponding classifier’s decisions. Also, answer the following question: Do you see any limitations of this method of classification?
# Exercise 2
## A
Run an experiment to investigate the effect of the number of hidden layers on the classification accuracy. Change the number of hidden layers to a low number, for example 1, and describe what happens. Next, change the number of hidden layers to a high number, such as 100 and describe what happens.
## B
Run an experiment to investigate the effect of the size of training data set on the classification accuracy. For example, take just a few samples for training and describe what happens. Make a small graph showing the accuracy vs training set size. Describe what you discovered. Is the graph linear? Can you notice a saturation trend? At what size of the training set did you notice almost no improvement in accuracy? Add also the MATLAB code you used.

---
References:

# Gantt
- Identifying risks/hazards -> Identifying requirements needed, wanted and not wanted
- Identify features and classes 
- Create training and test data sets
- Creating high level design of identification and classification model
- Creating a state machine diagram (from identification to result)
- basic Model implementation -> Testing -> Improvements
- implement *wanted* features (Ex. different coloured pens/ different handwritings)

image --> turn into csv file with as many rows as objects and columns as pixels per object image with label in first column

An image is subdivided into blobs and the image pixels are stored in an array. All blob arrays are stored in one map which links to it's coordinates which the NN loops over.

The output arrays indicate what character the blobs are.

- fractions $$\frac{a}{b}$$
- square roots $$\sqrt{4}$$
- limits $$\lim_{x\to 9}$$
- sum $$\sum\limits_{x=0}^{5}x$$
- sub and superscripts $x_{2}^{3}$ 
- braces use \left and \right
- matricies 
  $$
	   \begin{bmatrix}
	   1 & x & x^2 \\
	   1 & y & y^2 \\
	   1 & z & z^2 \\
	   \end{bmatrix}
	$$


-> we always use braces

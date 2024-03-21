---
Tags: 
Created: 2024-03-19 02:57:27
---
(Links:: [[Human Computer Interaction]])
# Models & Theories
## Descriptive Models
- provide a basis for discussing, understanding, reflecting, and reasoning about certain facts and interactions
- provide a conceptual frameword that simplifies a system
- are used to inspect an idea or a system and make statements about their probable characteristics
- used to reflect on a certain subject
- can reveal flaws in the design and style of interaction
## Predictive Models
> [!definition]
> Equation that predicts outcome (probablity of an outcome) based on the value of one or more predictor variables

- Useful to *explain* findings in past user experiments (model fitting)
- Useful to *predict* findings in future experiments (model validation)
- Equation can be simple (e.g. linear regression) or very complex
## Fitts' Law
- Developed by Paul Fitts in 1954
- Robust model of human psychomotor behavior
- Predicts movement time for rapid, aimed pointing tasks
- Fitts' descovery "was a major factor leadint to the mouse commercial introduction by Xerox"
- The time to acquire a target is a function of the distance to and the size of the target and depends on the particular pointing system

$$MT=a+b\times \log_2\left(1+\frac{D}{W}\right)$$
MT: movement time
a, b: constants dependent on the pointing system
D: distance to the target area
W: width of the target

- Index of Difficulty, $ID=\log_2\left(1+\frac{D}{W}\right)$
	- $MT=a+b\times ID$
	- ID describes the difficulty of the task independent of the device/method
- Units
	- Constant $a$ measured in seconds
	- Constant $a$ measured in seconds / bits
	- ID measured in bits

- Extension to 2D
- "Status Quo": use horizontal width
- "Sum Model"

---
References:

---
Tags: 
Created: 2024-03-19 02:57:27
---
(Links:: [[Human Computer Interaction]])
# Descriptive Models
- provide a basis for discussing, understanding, reflecting, and reasoning about certain facts and interactions
- provide a conceptual frameword that simplifies a system
- are used to inspect an idea or a system and make statements about their probable characteristics
- used to reflect on a certain subject
- can reveal flaws in the design and style of interaction
# Predictive Models
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
- "Sum Model": W = width + height
- "Area Model": W = width * height
- "Smaller Of": W = (width, height)
- "W Model": width in movement direction

**Linear Regression**
- How to measure $a$ and $b$ for a new pointing device / menu / etc.?
  $$MT=a+b\times \log_2\left(1+\frac{D}{W}\right)$$
  $$ID=\log_2\left(1+\frac{D}{W}\right)$$
- Setup an experiment with varying $D$ and $W$ and measure $MT$
- Fit a line through the measured points: $a$=intercept, $b$=slope
## Hick's Law
- The time needed to make a selection is proportional to the log number of alternatives given
- $T=b\times H$
- $H$ is the information-theoretic entropy of a decision
- $H=\log_2(n+1)$
  $n$ alternatives of equal probability
- Common practical values: $b=150 ms/bit$
- Hick's law does **not** apply if it requires linear search. It applies if the user can search by sub-division
## GOMS Model
- **G**: goals
	- (Verbal) description of what a user wants to accomplish
	- Various levels of complexity possible
- **O**: operators
	- Possible actions in the system
	- Various levels of abstraction possible (sub-goals/.../keystrokes)
- **M**: methods
	- Sequences of operators that achieve a goal
- **S**: Selection rules
	- Rules that define when a user employs which method (among alternatives)
- Users tasks are split into goals which are achieved by solving sub-goals in a divide-and-conquer fashion

> [!failure] Problem with GOMS
> - Only for well defined routine cognitive tasks
> - Assumes statistical experts
> - Does not consider slips or errors, fatigue, social surroundings,...

> [!success] Strengths of GOMS et al.
> - Gives reasons
> - Helps in decision making
> - Identifies bottlenecks
> - Provides illustrative figures
> - Combines various views
> - Treats feasibility and cognitive load
> - Less cost in money and time
> 	- Quick to apply
> 	- Quick to prepare
> 	- Helpful to design
> 	- Cheap to design
> 	- Easy to repeat
> 	- Quick to analye
> 	- Precise to interpret
> 	- Easy to convey

## **K**eystroke-**L**evel **M**odel
- Simplified version of GOMS
	- Only operators on keystroke-level
	- No sub-goals
	- No methods
	- No selection rules
- KLM predicts how much time it takes to execute a task 
- Execution of a task is decomposed into primitive operators:
	- Physical motor operators
		- Pressing a button, pointing, drawing a line, ...
	- Mental operator
		- Preparing for a physical action
	- System response operator
		- User waits for the system to do something
- Each operator is assigned a duration (amount of time a user would take to perform it)

| Operator         | Description                                                                     | Associated Time                                                                                                                            |
| ---------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| $K$              | Keystroke, typing one letter, number, etc. or function key like 'CTLR', 'SHIFT' | Expert typist (90 wpm): 0.12 sec <br> Average skilled typist (55 wpm): 0.20 sec <br> Average non-secretarial typist (40 wpm): **0.28 sec** |
| $H$              | 'Homing', moving the hand between mouse and keyboard                            | $0.4$ sec                                                                                                                                  |
| $B/BB$           | Pressing/clicking a mouse button                                                | $0.1$ sec / $2\times0.1$ sec                                                                                                               |
| $P$              | Pointing with the mouse to a target                                             | $0.8$ to $1.5$ sec with an average of $1.1$ sec <br> Can also use Fitts' Law                                                               |
| $D(n_D,I_D)$     | Drawing $n_D$ straight line segments of length $I_D$                            | $0.9\times n_D+0.16\times I_D$                                                                                                             |
| $M$              | Subsumed time for mental acts: sometimes used as 'look-at'                      | $1.35$ sec ($1.2$ sec according to [Olson and Olson])                                                                                      |
| $R(t)$ or $W(t)$ | System response (or 'work') time, time during which the user cannot act         | Dependent on the system, to be determined on a system-by-system basis                                                                      |
  



> [!example] Predicting the Task Execution Time
> - $OP$: set of operators
> - $n_{op}$: number of occurrences of operator OP
> 
> Example task on Keystroke-level:
> 1. hold-shift (*$K$*)
> 2. n $\times$ cursor-right (*$n\times K$*)
> 3. recall-word (*$M$*)
> 4. del-key (*$K$*)
> 5. n $\times$ letter-key (*$n \times K$*)
> 
> Operator Time values: $K=0.28s$ and $M=1.35s$
> $$2n\times K+2\times K+M=2n\times 0.28 + 1.91s$$
> -> Time it takes to replace a $n=7$ letter word: $T=5.83$ sec

> [!example]- Currency conversion
> ![[39927.png|500]]
> Convert 712 GBP into EUR (Hand is on the mouse at beginning)
> # Steps
> 1. Click into input field
> 2. Enter 712
> 3. Click GBP (source)
> 4. Click EUR (source)
> 5. Click "Perform Currency Conversion"
> # Actions
> - $P$ [to input field]
> - $BB$ [click]
> - $H$ [to keyboard]
> - $M$ [consider number]
> - $4\times K$ [BACKSP-7-1-2]
> - $H$ [to mouse]
> - $M$ [consider number]
> - $P$ [to GBP]
> - $BB$ [click]
> - $M$ [consider number]
> - $P$ [to EUR]
> - $BB$ [click]
> - $P$ [to convert]
> - $BB$ [click]
> - $R$ [show page with result]
> # Result
> $$T=4\times P+8\times B+2\times H+3\times M+4\times K+ R$$
> $P\approx 1.1s$
> $B=0.1s$
> $H=0.4s$
> $M=1.35s$
> $K=0.28s$
> $R$ (System dependent)
> $$T=4.4s+0.8s+0.8s+4.05s+1.12s+1s=12.17s$$

> [!example]- Start app
> # Application launchers
> Hand rests on mouse
> ![[2202.png|300]]
> ![[53072.png|300]]
> ![[61242.png|300]]
> ![[97564.png|300]]
> # Results
> 1. $$T=M+2\times P+4\times B+R=4.95s$$
> 2. $$T=M+P+2\times B+R=3.65s$$
> 3. $$T=2\times M+3\times P+6\times B+R=7.60s$$
> 4. $$T_1=M+2\times P+4\times B+2\times H+5\times K+R=7.15s$$
>    $$T_2=M+P+2\times B+H+6\times K+R=5.73s$$

> [!tip] Using KLM
> - KLM can help **evaluate UI designs**, interaction methods and trade-offs
> - If common tasks take **too long** or consist of too many statements, **shortcuts can be provided**
> - Predictions are mostly remarkable accurate: +/- 20%
> - Sometimes already the comparison of the number of occurrences of the different operators for different designs **reveal the difference**
## GOMS vs. KLM

| GOMS                                                                                   | KLM                                                                     |
| -------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Pseudo-code (no formal syntax)                                                         | Simplified version of GOMS                                              |
| Very flexible                                                                          | Only operators on keystroke-level <br> -> focus on very low level tasks |
| Goals and subgoals                                                                     | No multiple goals                                                       |
| Methods are informal programs                                                          | No methods                                                              |
| Selection rules <br> -> tree structure: use different branches for different scenarios | No selection rules <br> -> strictly sequential                          |
| Time consuming to create                                                               | Quick and easy                                                          |


---
References:

---
Tags: 
Created: 2024-03-26 02:42:54
---
(Links:: [[Human Computer Interaction]])

> [!definition] Prototype
> A concrete representation of a part or all of an interactive system

> [!question] Why do we prototype?
> - Minimize the time for early design iterations
> - Make errors quickl!
> - Enables to explore more design options

![[56003.png|500]]

# Horizontal vs Vertical Prototypes

| Horizontal                                                                                      | Vertical                                                                                                                           |
| ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Demonstrates the feature spectrum without implementation                                        | Demonstrates a specific feature                                                                                                    |
| Helps to evaluate: Navigation, Overall user interface concept, feature placement, accessibility | Helps to evaluate: optimal design for particular function, optimize usability of this function, user performance for this function |
| Applicable in low-fidelity prototyping and high-fidelity prototyping                            | The details of the function/feature are shown/implemented                                                                          |
| Used in early design stages (features to include and decide user interface concept)             | Often used in high-fi prototyping but is also applicable to low-fi prototyping                                                     |
# Low-Fidelity vs. High-Fidelity Prototypes

| Low-Fidelity                                                                                                                                                          | High-Fidelity |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| Check ideas and interaction flow                                                                                                                                      |               |
| Fast, cheap and easy to change                                                                                                                                        |               |
| **Advantages**: <br> - Cheap, easy and quick to implement <br> - Users are keen to criticise                                                                          |               |
| **Disadvantages**: <br> - No real functionality, difficult to identify errors <br> - Reuse and extending difficult to impossible <br> - Not all ideas can be realized |               |

## Low-Fidelity
### Advantages of Paper prototyping
- Paper prototypes  
	- Are cheap and easy to do  
	- Allow very quick iterations  
	- Help to find general problems and difficult issues 
	- Provide means for early “testing” with real users 
	- Enable non-technical people to interact easily with the design team (no technology barrier for suggestions)
- Make mistakes on paper and make them before you design your architecture and before you do the coding
- Helps to structure the information architecture and hence should lead to better system design

### Difficulties of Paper prototyping
- Feedback depends strongly on how the prototype is explained and handled
- Only specific use cases can be tested
- For technical people it is often hard to believe that it works (needs convincing to engage with paper prototypes)
- Limits what can be prototyped (architecture, physical prototyping, 3D)
- Limits what can be tested (e.g. the haptic feedback of a new gesture interaction can not be tested)
## High-Fidelity
# Non-Functional vs. Functional Prototypes
# Evolutionary prototypes vs. throw away prototypes
# Input Technology
> ...basically, an input device is a transducer from the physical properties of the world into the logical parameters of an application

There are a variety of different [[Human Factors#Basic Input Operations|input operations]] that describe the design space.

> [!tip] Four Criteria to assess input devices
> 1. Continuous vs discrete?
> 2. Agent of control (hand, foot, voice, eyes, ...)?
> 3. What is being sensed (position, motion or pressure)?
> 4. The number of dimensions being sensed (1, 2 or 3)

**Dimensions**
- 1D/2D/3D

**Direct vs. indirect**
-> integration with the visual representation
- Touch screen is direct
- Mouse is indirect

Discret vs. continuous
-> one single information vs. a sequence of information (in x DOF) interpreted as one input action
- Push button is discreet
- Mouse is continuous (mouse's position, though digitized into a discrete quantity, is fast enough to be considered continuous)

Absolute vs. Relative
-> improvement/position used as input
- Touch screen is absolute
- Mouse is relative

## Design Space
**Footprint**
- Size of the devices on the desk  

**Bandwidth**
- Human – The bandwidth of the human muscle group to which the transducer is attached  
- Application – the precision requirements of the task to be done with the device  
- Device – the effective bandwidth of the input device
## Controller Resistance
Isometric
- Device/handle is not moved
- pressure devices / force devices
- Infinite resistance
- device senses force but does not perceptibly move
- force is mapped to rate control of the cursor (cursor velocity) or to absolute position (force applied)

Isotonic
- Device/handle is moved
- displacement devices, free moving devices or unloaded devices
- constant resistance
- displacement of device is mapped to displacement of the cursor 

Elastic
- Device/handle is moved
- Device’s resistive force increases with displacement Device can sense displacement or force
- Force/displacement is mapped to rate control of the cursor (typical) or to absolute position
## Transfer function
Position control
- device displacement is mapped/scaled to position (typically for free moving/isotonic devices, also for elastic devices)
- absolute force is mapped/scaled to position (for isometric or elastic devices)

Rate control
- force or displacement is mapped onto cursor velocity 
- Integration of input over time -> first order control

---
References:
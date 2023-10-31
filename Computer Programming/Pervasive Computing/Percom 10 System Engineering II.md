---
Tags: 
Created: 2023-10-31 02:40:53
---
(Links:: [[Pervasive Computing]])

For a system to work seamlessly, the user needs to be *familiarized* with the technology, but also trust and accept the system. For many, they can only trust the system when it:
- functions as expected
- is safe
- is ok from an ethical point of view
- guarantees a good privacy
- is secure
- is easy to learn
- is attractive

These are all *quality* requirements and are considered nonfunctional.
Bugs occur in devices and their software all the time, which can lead to destruction and sometimes even fatality. Most of these bugs stem from the requirements of the system, not the design or code.
Systems are built and used by humans which will always make errors. All we can do to improve the quality is to test the system on it's requirements, design and code.
![[Testing Principle.canvas]]
# Testing for functionality
> [!question] Does the system do what is expected to do?

We can't test for *every* case. As such we must make a **selection** of all possible test cases.
## Boundary Values Analysis
A surprising number of faults a caused by **boundary values**. This method however only makes sense when the input taken is from a range.
> [!example]- Boundary value
> Consider this to be the correct implementation of code:
> ```python
> if(x <= 45) then f1 else f2
> ```
> If a developer were to have written 
> ```python
> if(x < 45) then f1 else f2
> ```
> instead, then the boundary value 45 would cause an error.

Generally the rule for one input variable is to test the cases in the range $x_{min}\to x_{max}$ for:
- $x_{min}$
- $x_{min}+1$
- Some value in the range
- $x_{max}-1$
- $x_{max}$
# Testing for safety
> [!question] Is the system safe?

Test scenarios for safety can be generated from a hazard analysis or from accidents analysis.
## Safety science concepts

- **Safety** is freedom from accidents or losses 
  – more realistically: an acceptable level of risk while preserving the benefits  
- A **hazard** is a condition with the potential for causing or contributing to an accident. 
- **Causal scenario**: What makes a hazard possible.  
- **Test scenario**: Brings the system in a dangerous state and checks how the system reacts.
## Test cases from [[Percom 9 System Engineering I#Hazard analysis|hazard analysis]]
Hazards can be identified with the [[Percom 9 System Engineering I#System Theoretic Process Analysis|STPA method]]. Once we have found the hazards we can take one and find the **causal scenario**. We then create a **corrective measure** to prevent this from happening again. After that we can test for the causal scenario again and determine if the corrective measure worked.

> [!example] Oosterschelde Flood barrier
> > [!question] What accidents can happen?
> > 1. People and animals die in a sea flood
> > 2. Assets (houses, roads, cars,...) are destroyed and lost in a sea flood
> > 3. Salted water shellfish die because the connection to the sea got closed
> 
> We can then construct a fitting model using STAMP
> ```mermaid
> 
> flowchart TD
> 	subgraph Controller
> 	a1["`if (level > 3m) then close doors`"] 
> 	a2[Process Model]
> 	end
> 	b1["`**Controlled process**:
> 	Sluice Gate Door`"]
> 	Controller --control action--> b3[Motors control system]
> 	b3 --> b1
> 	b1 --feedback--- b2
> 	b2[Water level sensor] --> Controller
> ```
> We then use the method described above to test the system
> - **Hazard**: The water is 4m high and one door is open
> - **Causal scenario**: A sensor wire is broken and makes the controller *think* that the water level is 0m
> - **Corrective measure**: The decision to open the door should rely on two sensors. Add second sensor
> - **Test case**: Simulate a broken sensor and check that the door also closes and a sensor malfunction alarm is generated.

---
References:
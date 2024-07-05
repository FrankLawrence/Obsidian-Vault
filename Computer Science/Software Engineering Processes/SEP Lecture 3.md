---
Tags: 
Created: 2024-06-24 15:25:55
---
(Links:: [[Software Engineering Processes]])
# What is Software Quality
> [!definition]
> The capability of a software product to satisfy stated and implied needs when used under specified conditions

> [!definition]
> Software quality depends on the degree to which those established requirements accurately represent stakeholder needs, wants, and expectations.

- High quality software meets its requirements, which in turn should accurately reflect stakeholder needs.
- Quality is about aligning the software with both its formal requirements as well as true user needs.
# Errors, Defects, Failures
- **Error**: A human mistake made by a developer 
  (e.g. Misunderstanding a requirement and coding to the wrong specification)
- **Defect**: A flaw or imperfection inserted into a software due to an error 
  (e.g. A bug in the code or issues with other artifacts like requirements. Defects get inserted when errors are made)
- **Failure**: The termination of the software's ability to function as intended.
  (e.g. When the software executing encounters a defect/The user experiences the software failing in some unintended way)
# Software Quality Management
- Quality planning: Defining quality objectives, requirements, targets, and planning of quality assurance
- Quality control: Techniques to measure quality characteristics, review work products, and find defects
- Quality assurance: Processes to ensure compliance with procedures
- Quality improvement: Defect analysis and process enhancements
- Resources: Infrastructure, tools, training that enable quality processes
- Standards: Regulations, models, certifications that guide quality work
- Culture: Values, behaviors that encourage quality mindset
# Software Evaluation
> [!question] What is Evaluation?
> The making of a judgement about the amount, number, or value of something
> Assessment, judgement, rating,...

> [!question] How is software evaluated? 
> Various points of view:
> 	- Functionality
> 	- Performance 
> 	- Quality
> 	- Usability
> 	- Maintainability
> 
> Various approaches:
> 	- Quantitative 
> 	- Qualitative
# Software Metrics
> [!question] What represents a good software metric?
> 1. Comparability
> 	- Allows comparison between software systems
> 	- Metric values can be compared
> 2. Intuitive interpretation
> 	- Concerns what the metric tells you about the system
> 	- A metric should be interpreted also by a non-expert in the domain
> 3. Simple and efficient computation
> 	- Compute the metric with little effort

> [!example] Chidamber and Kemerer Metrics Suite
> - Metrics for OO-software
> - Weighted Methods per Class (WMC)
> - Number of Children (NOC)
> - Coupling Between Object Classes (CBO)

> [!example] McCabe Cyclomatic Complexity Metric
> - A quantitative measure of independent paths in the source code of the software product
> - An independent path is that path which has at least one edge that has not been yet traversed in any other path
# Quality Attributes
## Performance Efficiency
- Represents the degree to which a software performs its functions within specified time and is efficient in the use of resources (such as CPU, memory, storage, network devices energy, materials,...) under specified conditions
- Time behavior: Degree to which the response time and throughput rates of a software, when performing its functions, meet requirements
- Resource utilization: Degree to which the amounts and types of resources used by a software, when performing its functions, meet requirements
- Capacity: Degree to which the maximum limits of a software parameter meet requirements
# Code Smells and Refactoring
> [!definition] Code Smell
> Indicators of potential issues (deeper problems) in the code over time

- Results of poor or misguided programming (not conforming to SOLID principles)
1. **S**ingle Reponsibility
2. **O**pen/Closed
3. **L**iskov Substitution
4. **I**nterface Segregation
5. **D**ependency Inversion


> [!example] Examples of Code smell
> - Large/God class
> - Long Method
> - Duplicated Code -> functions
> - Switch statement -> interfaces/inheritance/abstract class
> - Feature Envy -> Create multiple classes
> 
> > [!example]- Large class code smell
> > ```cpp
> > public class Student { 
> > 	private String name, surname; 
> > 	private String streetName; 
> > 	private int streetNumber; 
> > 	private int grades[]; 
> > 	public void Student () {...} 
> > 	public boolean addGrade (int grade) {...} 
> > 	public boolean removeGrade (int grade) {...} 
> > 	public void listGrade () {...} 
> > 	public boolean changeAddress (String newName, int newNumber) {...} 
> > 	... 
> > }
> > ```


---
References:
---
Tags: 
Created: 2023-03-23 13:15:44
---
(Links:: [[Computer Organization]])
# State Diagram
```mermaid
graph TD;
A[Product idea] --> B[Design product]
B --> C[Consider alternative implementation]
C --> A
B --> D[Design of development plan]
D --> C
D --> T[Assess budget]
T --> E[Choose platform]
E --> F[Create campaign]
F --> G[Idea advertisement/promotion]
H[Funding goal not reached] --> I[Propose removed/changed/new features]
P --> L[Goal reached]
L --> Q
N[Customer feedback] --> I
K[Product finished] --> O[Product launch]
S --> N
H --> G
G --> P[Funding]
I --> Q[Product development]
Q --> K
P --> H
K --> R[Beta testing]
R --> I
O --> S[Product Promotion]
U[Inform backers] --> I
Q --> U
```
- Introduction
	- detailed description of the process
	- specify the main character
- Transition State Diagram
	- Diagram 
	- What is you model M?
	- what is your set of finite state S?
	- what is your initial state So.
	- What is your set of transitions TR?
	- Explanation.
- Petri Net
	- Petri net diagram
	- Formal Description
	- Explanantion
- Reachability Graph
	- graph
	- initial state
	- terminal state
	- existence of any circular loop
	- problems
- Analysis
	- describe all the properties (terminating, boundedness, safety, deadlock freedom, dead transition, liveliness, home marking and reversibility.)
- Reflection
	- Why is the model useful?
	- Disadvantage (limitation)
	- Group Contribution.
# Petri Net
Here are some additional steps that could be part of the process for developing a new product:

1. Conducting market research: Before creating an initial idea, the team could conduct market research to identify gaps in the market or areas where there is high demand for certain types of products. This can help inform the development of a product that is more likely to be successful.
2. Creating a prototype: After the design team has developed a proposed design, they could create a prototype of the product to test its functionality and usability. This could involve creating a physical model or a digital prototype, depending on the nature of the product.
3. Testing and quality assurance: Once the development team begins developing the product, they should conduct thorough testing and quality assurance to ensure that the product is functioning properly and meets the desired specifications. This could involve testing the product under various conditions, such as different environments or user scenarios.
4. Manufacturing and distribution: Once the final product has been completed, it needs to be manufactured and distributed to retailers or directly to customers. This could involve coordinating with manufacturers and logistics companies to ensure that the product is produced and delivered efficiently and cost-effectively.
5. Post-launch monitoring and support: After the product has been launched, the team should monitor its performance and gather feedback from customers to identify any issues or areas for improvement. They should also provide ongoing support to customers, such as technical assistance or troubleshooting, to ensure that they are satisfied with the product.
# Transition state diagram 2
```mermaid
graph TD;
A[Product Idea] --> B[Product blueprint]
A --> A
B --> A
B --> C[Development planned]
C --> D[Budget created]
D --> E1[Equity]
D --> E2[Reward]
D --> E3[Donation]
E1 --> F[Platform chosen]
E2 --> F
E3 --> F
F --> G1[Marketing team hired]
F --> G2[Campaign created]
G1 --> G2
G2 --> H[Campaign promoted]
H --> I1[Funding reached]
H --> I2[Funding unsuccessful]
I2 --> A
I2 --> G2
I1 --> J[Start development]
J --> K[Inform backers]
K --> L[Beta prodcut]
L --> K
K --> M1[Reward backers]
K --> M2[Product release]
M1 --> M2
```

---
References:
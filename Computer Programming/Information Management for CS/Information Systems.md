---
Tags: 
Created: 2023-02-21 17:56:41
---
(Links:: [[Information Management for CS]])
```ad-question
collapse: none
- understand how the role of information systems has changed over the past decades 
- understand of which importance information systems are today.
```
# Introduction
- decline in maufacturing and rise of employment in service and information sectors
	- development of faster, cheaper and more flexible technology for information processing and information transmission -> information sector grows
	- complex and volatile market forces means that business requires more targeted and more current information to gain a competitive advantage and survive
## Data and Information
```ad-definition
title: Information
collapse: none
- Information is data processed for a purpose
	- it helps aid some kind of decision
- decisions are taken by decision takers who decide based on organizational objectives, background information and personal interests
```
- transaction data can be recorded and undergo some sort of processing so the results are communicated for a particular purpose
- Ex: buying product and saving transaction data
	- information derived from data is used for making decisions (like checking if credit-limit is exceeded), of which planning decisions and control decisions are the most important
## Data processes
| *Type of data process*          | Example                                                                                                                        |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| classification of data          | Transaction data may be classified as invoice data, payment data, order data                                                   |
| rearranging/sorting data        | Data on employess may be ordered accoring to ascending employee number                                                         |
| summarizing/aggregating data    | Data on the performance of various departments may be aggregated to arrive at a summary of performance                         |
| performing calculations on data | Data on the total hours worked by an employee may be multiplied by the hourly wage rates to arrive at a gross wage             |
| selection of data               | Total yearly turnover data on customers may be used to select high-spending customers for special treatment by sales personnel |
# Decisions
## Cognitive style and background
- Cognitive style: the way that individuals absorb information, process it and relate it to their existing knowledge to make decisions
- styles of absorbing information
	- detailed: various elements of informaiton need not be linked as a whole
	- holistic: less concrete, general facts, suppositions and 'soft data' linked as a whole
- styles of decision making
	- Analytical: detailed justification involving quantative reasons for final decision
	- intuition: experience and rules of thumb judgment; situation as a whole 
- information presented in a way that is not conducive to cognitive style of the recipient will not be fully utilized in a decision
- information systems must take into consideration the different cognitive styles of those whom the information is provided 
- decision takers judge different aspects of information as more/less relevant or important for making decision (based on subject specializations)
## A model of decision making
1. **Intelligence:** The decision maker needs to be made aware of the existence of a problem that requires some sort of decision. Information needs to be presented in a manner conducive to this recognition
2. **Design:** Alternative solutions to the problem must be considered. This invloves the recognition of the range of acceptable decisions and the implications of each. At this stage, information needs to be supplied to aid the decision maker in predicting and evaluating these implications
3. **Choice:** This involves the choice between the various alternatives investigated in the previous stage. If there has been a full analysis of the positions this should be a straightforward stage. Otherwise, the decision maker may have to choose between incomplete and perhaps incomparable alternatives
4. **Implementation:** The chosen decision is carried out
- [[Decision making example]]
## Levels of managerial decision taking
- **Strategic planning**
	- carried out by most senior management 
	- broad issues concerning organization's development over the long term
		- Ex: diversify production, move into different market, structure company's finances, investment projects, contracts, or organizational objectives
	- information for decision relates to future not present
	- external information from market surverys, trade publications demographic studies, government reports and research from specialst suppliers
	- more uncertain than detailed
- **Tactical planning and control**
	- allocation of resources within departmental budgets
	- medium-term work scheduling and forecasting
	- planning medium-term cash flows
	- monitoring of production and expenditure against budgets
	- now and next few months
	- information generated internally (some external, ex: price of raw materials)
	- information not as consolidated as for a strategic decision
	  -> less uncertain
- **Operational planning and control**
	- decisions in day-to-day operations
	- effective and efficient use of existing resources to realize budget objectives
	- treatment of personnel, control of inventory and production levels, pricing decisions, accounting and cash controls
	- information generated exclusively internally
	- highly detailed, certain and immediately relevant
- ![[Information characteristics for managerial decisions.canvas]]
## The structure of decisions
- **Structured decisions:** governed by clear rules
	- set of steps to be followed in decision table or procedural logic flowchart
	- information needed is clearly specifiable and unambiguous
	- process of arriving as decision is straightforward
	- can be left to low-grade personnel or fully automated in some cases
- **Unstructured decisions:**
	- unclear of information needed and how to be assessed in relation to decision objectives
	- objectives may be unclear
	- no set procedure 
	- use of heuristics and 'experience'
- [[Structure and managerial activity in decision making]]
# Management information systems
- stored transaction data could provide management with useful information
```ad-definition
title: Management information system
collapse: none
Any system that provides information for the management activities carried out within an organization
```
- Reasons for increased growth in management information systems
	- *Cost*: cost for generating information based on transaction data is low
	- *Speed*: information can be generated quickly; information provided is up to date and decisions should be more effective
	- *Interaction*: users may request information to be produced on-line as and when it is needed
	- *Flexibility*: predictable set of decisions for which information is required, *and* flexibility to enable manager to decide what information is to be produced
-  Alternative terminology
	- Executive information systems: used by senior management to assist in strategic decision making
	- Management information systems: facilitate routine summarizing and reporting
	- Decision support systems: allow *ad hoc* queries and analytical reporting
	- Transaction processing systems: payroll, order tracking, machine control and employee records
## Database
- data needs to be managed as a central resourse
- database managmenet system: software that creates database and handles access to it
- dms handles the programs controlling the data processing and the database
- database is protected from direct contact with applications programs
- ![[The provision of information from a database.png|500]]
### Models
- predicts the cash flow position of the company
- predictive model will need data that will not be in the database (inflation and growth in market sizes)
- decision support system: modelling element of provision of information is designed to aid decisions
## Management information systems as a collection of subsystems
- individual information subsytems loosely connected -> MIS is a collection of information subsystems, sharing a corporate database
- Each subsystem tailord to needs of the functional subsystem that it servers
- MIS tend to evolve over time -> too complex to design the unified system as an initial project
## Design of the management information system
- **Relevance:** information generated by MIS must be relevant to the tasks for which it is provided
	- analysis of decisions taken to undersatnd what information is relevant
- **Accuracy:** important for correct decisions -> increase cost in provision, slower generation or both
- **Timeliness:** Information must be presented to the user during the time-span within which it is useful (accuracy maybe sacrificed)
- **Target:** information correctly transmitted to recipient (decision maker)
- **Format:** presentation of information to the user (graphical, charts, color); presentation adjusted to cognitive style of the recipient
- **Interactive nature:** further information can be called up only if shown to be neccessary by current information; information can deluge decision taker and reduce effectiveness of decision process
- **Control:** information can be of value to competitors -> secure
```ad-note
title: Dos and don'ts in MIS design
collapse: none
- less information should be provided rather than more -> relevant information might get ignored 
- don't ask the decision maker what information is needed, make an analysis of the decision
- management accountant needs not only the information but also knowledge of the way it is produced
```
```ad-note
title: Approaches to management information system design
collapse: none
1. **By-product:** develop a computerized system to deal with all the paperwork
	- Payroll, accoutns receivable and accounts payable, stock control, billing
	- information produced consists of reports from which those that need information find it impossible to disentangle what is relevant
2. **Null:** activities undertaken are seen as dynamic and everchaning -> production for formal information by an MIS according to static requirements is inappropriate
	- lower management is clearly defined -> MIS serves correct information
	- interactive systems makes production of information according to dynamically changing requirement much easier
 3. **Key variable:** certain attributes of an organization are crucial for decision making
	 - MIS is designed to provide reports on the values of key variables given 
	 - *exception reporting:* report iff value of variable lies outside some predetermined 'normal' range
	 - it's effective if information is provided selectively
4. **Total study:** comparison between the information requirements of management and the information supply of the current management information system 
	- interview managers for key decisions, objectives and information needs -> gain overall understanding of the organization's information needs and identify just where the current system is falling down
	- plan is formulated
	- can identify shortcomings
	- expensive manpower and alot of data to be analysed
	- bias may occur
5. **Critical success factor:** certain goals and specific factors curcial in achieving goals for an organization
```
- critical success factors (local competitors, geographical position, history of company) show where good management information is needed -> information subsystems designed to serve these critical factors
- CSF: active approach to the design of management information systems rahter than the passive acceptance of reported information based on tradition and collected historical data
- CSF is information rather than data-led
- **CSF recognizes that the purpose of providing information is to serve corporate goals**
## Production of the management information system
- third party MIS purchased by organization and adapted based on own business knowledge

---
References:
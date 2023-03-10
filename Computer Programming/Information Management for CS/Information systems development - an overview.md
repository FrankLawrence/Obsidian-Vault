---
Tags: 
Created: 2023-02-23 12:16:40
---
(Links:: [[Information Management for CS]])
```ad-question
collapse: none
- [[Information systems development - an overview#Reasons for project initiation|When and why do organizations start system development projects?]] (Also be able to relate the content of the book to the “Make or buy”-discussion from the lecture)
- [[Information systems development - an overview#The participants in analysis and design|What are the participants in system development projects and which challenges occur due to this interaction?]]
- [[Stages of the life cycle|What are the stages of the systems life cycle?]]
```
# The need for systems analysis and design
- it's easy to implement computer-assisted support on small business (needs are clearly definded) -> unsatisfactory for developemnt of complex system
- the larger the organization the more complex and individual are the data-processing and information needs
- requirements of users must be identifiend and suitable system designed to meet those needs
- must be controlable, secure, flexible for future
## The need for an information systems strategy
- needs of users have evolved rapidly
- no development of independent internal information systems -> control by way of a well-worked-out information systems strategy
	- find business activities important to computerized systems development -> map out plan for development of projects
	- incorporate new developments in technology and future needs
	- strategy for centrally controlled development of projects and for local developments to meet local needs
- projects need to be coordinated with one another as well as ensuring that each is internally well organized
## Information systems steering committess
- form overall development strategies and allocate resources
- aim to ensure that the information systems in the organization deliver an effective service with cost efficiencey
- **recommends an overall policy for information and data-processing systems development**: e.g. centralized or decentralized system, data protection legislation, standardized computer equiptment
- **ensure that individual user department needs are being satisfied**: individual user department managers enusre user views are articulated
- **initiate and monitor individual projects**: scope and objectives of each project, setting up project team, receiving progress reports
- **coordinate individual projects**: projects that affect one another
- **report 'upwards' to top management**: summary reports on project development, present and future costs
- **be responsible for the appointment of senior personnel in the computer centre**: job specifications managed
## Reasons for project initiation
1. **Current system cannot cope**: 
	- previous system manual or based on computer
	- increase in volume of transactions processed -> too slow -> backlogs build up
	- change in organizational structure renders current system inappropriate
2. **Cost savings**: 
	- time-consuming manual work -> cheap computer substitutes (payroll and mass billing)
3. **Provision of better internal information for decision making**:
	- supply of fast accurate targeted information -> better decisions
4. **Provision of competitive customer services**:
	- better customer services for competitors to adapt 
5. **Opportunities provided by new technology**: 
	- outdated tech doesn't offer same range of facilities
	- networks, improvements in storage devices, processing power, internet
6. **High-technology image**:
	- image suffers from using outdated technology
7. **Changes in legislation**:
	- data protection legislation or alterations to taxation
8. **Balancing the portfolio of projects**: 
	- depending on risk and business impact, range of projects may be undertaken
## The participants in analysis and design
- users of existing system: provide information on current system -> specify in own terms the requirements of the new system
- Systems developers or programmers: turn requirements into programs
- Systems analyst or business analyst: 
	- understands and communicates with users to establish requirements
	- expert knowledge of computers
	- *frames requirements in terms that programmers can understand*
	- good communicator who can *think in terms of the user's point of view as well as that of the programmer*
 ```ad-example
 - the client (user) state his or her understanding of what the building should look like and what functions is should perform
 - the architect (analyst) takes the intentions and provides a general sketch of the building that will satisfy them (logical model of the intended system)
 - after agreement on sketch, detailed blueprint of design (detaild program specification) is designed for builders (programmers) to work with
 ```
 - responsibilites of analyst:
	 - investigating and analysing the existing system ast to its information use and requirements
	- judges whether it is feasible to develop a computer system for the area
	- designs new system, specifying programs, hardware, data, structures and control
	- testing and overseeing the installation of the new system and on it's performance
	- good communicator with business personnel and with technical staff
		- handles conflicts of interest
		- managerial and project management skills
		- process isn't mechnical -> creative
		- confidence and controlled enthusiasm
# The need for a structured approach to analysis and design
## A structured approach
- analyst builds logical model of the system from description of existing system
  physical details ignored:
	- media on which data is stored
	- organization of the data stores
	- who or what carries out data processes and where
- ![[A model of the process of analysis and design.png|500]]
# The life cycle of a system
- at the stage end 'deliverable' documentation proves the work carried out -> successful completion of the stage (exit criterion)
- linear development by stages, with deliverables and exit criteria enables the projcet to be controlled and managed
	- subdivision of complex lengthy project into descrete chunks
	- parts of project are forced to reach the same point of development at the end of stage -> promotes coordination
	- historical trace of development with documentation
	- document deliverables help communicate between analyst, programmers, users and management
	- stages as natural division points in development
	- no need to spend large sums of money until previous stages have been completed
## Stages in the life cycle of a systems project
[[Stages of the life cycle]]
### Stage 1 Determination of scope and objectives
- analysts' initial terms of reference
- indication of overall scope of the project
- indicate an area to be investigated such as sales order processing
- specify date by which stage 2 is to be produced and budgeted cost allowable for this
### Stage 2 Systems investigation and feasibility study
- report on feasibility of technical solution to problems or opportunities
- solution(s) outline presented with costs, benefits and feasibility associated
- steering committee decides if to carry on with project
- analyst investigates current system for functions of new system
- analyst interviews users and views existing documentation
### Stage 3 Systems analysis
- build logical model of the existing system
- establish what has to be done in order to carry out the functioning of existing system
- decomposition of the functions of the system into their logical constituents and the production of a logical model of the process and data flows neccessary to perform these (process analysis)
- entities of interest used as data stand in relation to one another
	- entities and relationships are combined in data model of the organization (data analysis)
- outputs logical process model (data flow diagrams) and specification of the process algorithms, data dictionary and data model
### Stage 4 Systems design
- range of alternative designs to implement with different implications for cost, security, ease of use, maintainability and efficiency
- data stores in series of files or database? centralized or distributed database system? which processes are computerized and which manual
- design alternatives suggested to management (usually reflect cost of the solution)
### Stage 5 Detailed design
- detailed physical specifications
- Structured tools set specifications for programs -> programs developed to perform various functions required
- hardware requirements to perform tasks efficiently
- structure of database
- schedule for implementation of the system
- system must be designed to ensure maximum relia- bility in secure, complete, accurate and continuous processing
- user-machine interface designed
-> estimate costs accurately
### Stage 6 Implementation
- system physically created, hardware purchased, programs written and tested (together)
- database with old system data loaded
- procedure to govern operation of the new system designed 
### Stage 7 Changeover
- old system replaced or run in parallel
- systems compared
- small version of system run before full implementation -> identifies errors
- 'phase in' introduces functionality in staged fashion
### Stage 8 Evaluation and maintenance
- transfer maintenance of hardware to manufacturer
- maintenance contract demanding conditions and charges for maintenacne
- existing programs are amended in light of changes in the requirements of users
- system evaulated and shortcoming rectified if any


---
References:
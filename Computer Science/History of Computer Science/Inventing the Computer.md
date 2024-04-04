---
Tags: 
Created: 2024-01-15 15:06:26
---
(Links:: [[History of CS]])

- ENIAC (electronic numerical integrator and computer) maintained by *five* female operators
- Adele Goldstine wrote it's programming manual and was married to the Army's project lead
- ENIAC first used for hydrogen bomb calculations
- Moore School tasked with design and use of ENIAC computer

![[ENIAC.jpeg|600]]

- It was the first *electronic* and *programmable* computing device that automated the job of deciding what to do next after a sequence of operations finished

> [!definition] Computer Program
> A configuration that carried out the operations needed for a job

- firing tables had to be computed for each configuration of the mortar and it's environment which took months to calculate for one single gun -> need for *automation*
- Penn and the Ballistics Research Laboratory tasked with with finding the capabilities ENIAC needed
- after calculating shell's trajectory used for nuclear weapon development
- data for ENIAC inform of punched cards: cardboard with 80 digits as a pattern of holes
	- some programs required rewiring of the computer and used thousands of intermediate cards
	- the women helped scientists and engineers to **formulate the problems** as sequences of ENIAC operations
	- later computers had *teams* for individual tasks: key punch women, computer operators and programmers
- ENIAC used twenty-eight vacuum tubes to hold each decimal digit -> successor EDVAC (electronic discrete variable automatic computer) used the **delay line**
# The EDVAC Approach
- [[John von Neumann]] added to ENIAC team -> first draft of the EDVAC: "the stored program concept" (instructions stored in same storage device as data)

## EDVAC paradigms
1. Hardware paradigm: all-electronic machine with a large high-speed memory using binary number storage
   ![[EDVAC logical architecture.png|500]]
	- ENIAC had many data pathways between units and adding circuits for every storage unit <-> EDVAC had *one connection* to memory and one **central arithmetic** organ
1. Von Neumann architecture paradigm: used biological language to describe architecture: *neurons* (switching circuits), *organs* (structure), *memory* (large delay line)
	- bits (contraction of *binary digits*)
	- all data was loaded into memory
3. System of instruction codes: flow of instructions and data mirrored that of scientific calculations

> [!info] Basic Cycle of EDVAC
> 1. Fetch next instruction code from memory -> copy to central control organ
> 2. Decode instruction
> 3. Execute it
> 
> - Program counter holds address from which the next instruction should be fetched -> increments by 1
> - Changing the counter causes a *branch* to a different part

- Klara von Neumann coded the first modern program: observing virtually and experimenting with the chain reaction inside an exploding nuclear weapon

## Variations on a Theme
- successful ideas were widely copied
- Manchester's *Mark 1* computer processor had an index register to simplify work with data structures
- a single memory location was a *word* of memory
	- a larger word size made working with bigger numbers convenient and efficient
	- methods were devised for storing short words whilst supporting larger numbers by using two words
- EDVAc was a *four-address* machine, meaning four instructions could be simplified into one in the final machine
- EDSAC (the first full-scale computer to implement all three of the EDVAC paradigms) was *serial* (data transferred over one line) and was slower but simpler
## Programming Tools
- Grace Hopper built up a paper tape library of standard sequences called *subroutines*
- automating program preparation lead to the development of **assemblers** that reused code by assembling subroutines and new code into a *single executable program*
- Humans started using abbreviations for instructions called [[mnemonics]] (used in [[Assembly]])
- assembly language: list of instruction mnemonics and parameters
- symbolic assemblers used labels instead of numbers for addresses -> no need to edit code when locations of variables changed
- macro instructions expanded into an entire block of code -> easier for retrieving information off of tape drives
# The Commercialization of Computing
- Howard Aiken didn't believe that electronic computers would be successful on the commercial market
- Eckert and Mauchly resigned from Moore School against giving up the rights to their work and founded a company
## The First Computer Start-Up
- The Eckert-Mauchly Computer Corporation had problems with getting resources and building the components for their clients
- The *Univac* was their main product
## Univac in Use
- commercial sales began in 1951 of large-scale stored program computers
- Univac was flexible enough to be used in business and scientific environments
- Univac started the era of "data processing" applications
- In the beginning only used by the Census Bureau for statistical purposes
- By using a tape drive instead of punched cards, it made it less labor-intense and replaced people tending to the computers
- The machine was commandeered by the US Air Force and the Atomic Energy Commission for urgent problems.
- Data initially punched onto eleven million cards was transferred to tape for processing by the Univac.
- Univac #2 was installed at the Pentagon for use in Project SCOOP, which played a key role in the development of linear programming.
- Univac #2 was put to work on SCOOP around the clock, but its uniprinter was a poor match for the high-speed tape and processor.
- Remington Rand delivered the Univac High-Speed Printer in 1954, which was able to print a full 130-character line at a time.
- Univac #5 performed classified weapons work, but it was also used to predict Eisenhower's victory over Adlai Stevenson in the 1952 presidential election.
- General Electric purchased a Univac for four specific tasks: payroll, material scheduling and inventory control, order service and billing, and general cost accounting.
- The GE Univac first produced payroll checks for the Appliance Park employees on October 15, 1954, which was a significant milestone.
- Univac #1 was used to process several monthly economic surveys throughout the 1950s, enabling the Census Bureau to disseminate data on magnetic tape.
## An Automated Utopia
- GE aimed to use the Univac to replace punched card machines and automate business processes.
- The Univac heralded an age of automation, as described by John Diebold in his 1952 book "The Automation Age."
- GE's installation of a Univac was the first industrial installation of an electronic computer in 1954.
- The Univac was used for various tasks, including long-range planning, market forecasting, and revamping production processes.
- The computer was promoted as the instigator of a digital utopia, transforming the nature of computing and its applications over time.

---
References:
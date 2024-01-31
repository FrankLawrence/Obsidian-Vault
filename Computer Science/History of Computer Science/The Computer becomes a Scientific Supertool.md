---
Tags: 
Created: 2024-01-16 01:37:21
---
(Links:: [[History of CS]])

- Cray 1 *supercomputer* had more than 1 million times the computational power of ENIAC
- high demand for well-funded Cold War laboratories and defense contractors

# The First Scientific Computers
- IBM announced it's first computer, the 701 ("Defense Calculator") in May 1952, used mostly be the US Defense Department and military aerospace firms, such as in Los Alamos
- used initially for weapons design, spacecraft trajectories, cryptanalysis
- also used for: logistics for a military agency, financial reports, actuarial reports, payrolls (for North American Aviation) and even predicting the results of a presidential election for network television (in 1956, the 701 correctly predicted Eisenhower’s reelection)
- IBM 704 used *magnetic core memory* which was **nonvolatile** and allowed for **random access**
## Interrupts and Channels
- All IBM computers till mid-1960s had the same architecture as the 704
- instead of constantly looping checking whether input or output operation is done, allow for *interrupts* by the I/O when data has been read or printed, and call subroutine 
  -> used in Univac 1103 and later IBM 704
- IBM 709 introduced input and output *channels* for high data throughput. It could without using the main processor:
	- print a file
	- copy data from one tape to another
	- load a file into memory

## Smaller Drum-Based Computers
- Smaller drum-based computers were cheaper alternatives to million-dollar computer systems like the Univac 1 or IBM’s rival 701.
- Magnetic drums were used as temporary memory in the first attempt to build an electronic digital computer in the late 1930s.
- After World War II, the drum re-emerged as a reliable, rugged, inexpensive but slow memory device.
- The leader in this effort was a Twin Cities, Minnesota, firm with its roots in a World War II codebreaking group called Engineering Research Associates (ERA).
- ERA’s engineers developed magnetic drum technology for the Navy, starting with a drum on which they had glued oxide-coated paper.
- Task 13, assigned in August 1947, was to build a programmable electronic computer, code-named Atlas, around the drum memory.
- In December 1951, ERA announced a commercial derivative, the Model 1101, which required much more capital than its founders could provide, and it was purchased by Remington Rand.
- Remington Rand could then offer two well-designed and capable computer systems: one optimized for science and engineering and the other for commercial use.
- Finding a reliable memory was by far the hardest part of putting together a computer in the early 1950s.
- ERA’s big hit was the drum, not the 1101 computer.
- ERA built drums ranging from 4.3 to 34 inches in diameter with capacities of up to 1 million bits, or 65,000 30-bit words, by 1949.
- Start-up companies built cheap and simple machines around these drums, including the Computer Research Corporation, the Electronic Computer Corporation, and Consolidated Engineering.
- These smaller computers accounted for a clear majority of those installed during the 1950s.
- The Librascope/General Precision LGP-30, delivered in 1956, had a repertoire of only sixteen instructions and looked like an oversized office desk. It needed only 113 vacuum tubes and 1,350 diodes, against the Univac’s 5,400 tubes and 18,000 diodes.
- The G-15, a drum-based computer designed by Harry Huskey and built by Bendix, was perhaps the only computer built in the United States to have been strongly influenced by Alan Turing’s ACE design.
- The LGP-30 and G-15 were difficult to program but very fast.
- The Bendix G-15 sold more than 400 machines, but it could not come up with a successful follow-up model.
- A July 1960 census of computers estimated that a total of 2,704 “medium-scale” computers such as the Bendix model had been installed in the US, greatly exceeding the 466 “large-scale” computers delivered.
# Scientific Programming Tools
- assemblers made writing machine instructions faster, *compilers* translated mathematical equations and understandable code written in *high-level languages* into computer code
- full-time programmers worked in data processing department, where programs where run multiple times -> need for operational efficiency
- scientific programmers were researchers and engineers who temporarily needed the computer in larger projects -> speed more important

## Early Compilers

> [!definition] Compiler
> "A program-making routine, which produces a specific program for a particular problem"
> ~ Grace Hopper

- "Automatic programming": Using compilers
- The first modern compiler translated mathematical equations into Whirlwind machine code
- Closing that gap between automatic compilers and hand coding was necessary to win acceptance of compiler systems

## Fortran
- "formula translation" became popular among IBM customers
- programming and debugging costed more than running the program -> focus on this field
- Syntax similar to that of mathematical equations
- clear, concise and easy-to-read user's manual
- fast and efficient code generated by compiler

## SHARE and the Operating System
- SHARE was a formal user group for the IBM 704 created in 1955 by a group of IBM 701 users in the Los Angeles area.
- SHARE allowed users to take on the work of supporting scientific computer users themselves, which hastened the acceptance of IBM's computers.
- SHARE developed an impressive library of routines that each member could use, many of them for mathematical tasks such as inverting matrices.
- SHARE was particularly important in shaping the growth of what was eventually called operating system software.
- The first utility programs to remain in memory while application programs ran were called monitors.
- SOS (Share Operating System) was the first piece of software to be called an operating system.
- IBM built on the SHARE routines to create a suite called IBSYS, which included a batch processing system designed to integrate with Fortran.
- IBSYS was based on FORTRAN Monitor System (FMS) and Bell Labs' "BESYS" rather than the SHARE Operating System.

## Mathematical Software
Here are the key takeaways from the article about high performance scientific computing and Fortran programming:
- High performance scientific computing depended on software as well as hardware.
- Fortran programming was an increasingly vital skill for scientists and engineers.
- Computer analysis and simulation underpinned scientific breakthroughs in various fields.
- To teach programming to large numbers of students, it was essential to find ways to compile their code efficiently.
- Watfor was a fast in-core compiler with good error diagnostics, which proved especially useful to students for debugging their programs.
- Eispack and Linpack were important early projects for producing reliable and efficient implementations of modern mathematical methods.

## Algol
- Algol was a hugely influential programming language that introduced the block structure and supported recursion.
- Fortran was a reasonably consistent language, but understanding the foibles of particular architectures was essential to maximizing performance.

## Toward the Supercomputer

Computers became popular in the commercial market due to their ability to automate administrative processes, which was of great interest to companies looking to optimize their efficiency. American businesses were quick to adopt computer technology, motivated by the idea that computers would transform business management. The administrative use of computers began in the United States in 1954, with General Electric being the first company to order a Univac for administrative use. The surge of computer installations was driven by a wave of enthusiasm for the revolutionary power of computer technology. Clerical automation soon overtook scientific calculation as the biggest market for computer technology. Lyons and Company, a British tea shop chain, was an early entrant to this market, establishing LEO Computers in 1954 to market computers to others. LEO sold dozens of computers to customers across Britain and its empire. They were powerful machines with some novel features, but in the end, LEO lacked the capital and international reach needed to compete with IBM.

IBM's entry into the commercial computer market was marked by the development of the IBM 702, a tape processing machine designed for business. It employed decimal arithmetic and variable word length to support accounting work, and its instruction set, addressing system, and arithmetic capabilities were tailored to the needs of business applications. Subsequently, IBM's 705, 7070, and 1401 models further solidified its presence in the commercial market, with the 1401 becoming a widely adopted and successful small business-oriented computer. The 1401's success was attributed to its flexibility, reliability, and the introduction of the type 1403 printer, which significantly enhanced its capabilities. By 1962, IBM was receiving more income from computers than from punched card machines, marking a significant shift in its business focus.

The importance of computers in business is evident in their essential role across various business operations, including product creation, marketing, accounting, and administration. Computers have significantly improved speed, accuracy, forecasting capabilities, connectivity, and information security in business processes. They have also streamlined communication, research, and media production, contributing to increased efficiency and productivity.

The history of computers, including their evolution and impact on business and government, is well-documented in various sources, such as "Computer: A History of the Information Machine" by Martin Campbell-Kelly, William Aspray, Nathan Ensmenger, and Jeffrey Yost. Additionally, the evolution of computers from the early 1800s to the present day, including their types and definitions, is a rich area of study.

---
References:
---
Tags: 
Created: 2023-02-04 00:22:35
---
(Links:: [[Computer Organization]])
# Study Goals
1. Explain the basic concepts, historical objectives, and modern functions of digital computers.
2. Describe the basic architecture and operation of digital computers.
3. Use proficiently binary data representation, number representation, and arithmetic and data conversion.
4. Explain at a proficient level the architecture and operation of each of the main components of a digital computer: the basic processing unit, the hierarchical memory system, the I/O system, and the interconnection system.
5. Explain at a basic level various system mechanisms for building faster single-node systems, such as pipelining and caching, and large-scale systems, such as interconnects and program synchronization.
6. Demonstrate proficiency with basic assembly programming by implementing basic operations of digital computing in realistic scenarios.
7. Analyze at a basic level the tradeoffs inherent in the design of digital computers, concerning among others performance (simple modeling), scalability (Amdahl's Law), availability, energy consumption, and cost. 
# Course schedule

| WeekOfYear | Date                | Time        | Session Type  | Session ID (Lecture ID) | Topic/Notes                                                    |
| ---------- | ------------------- | ----------- | ------------- | ----------------------- | -------------------------------------------------------------- |
| 6          | Mon 	Feb 06         | 15:30-17:15 | Lecture       | L1 (L0)                 | Course Overview + History of Computing + Entry quiz            |
|            | Tue 	Feb 07         | 17:30-19:15 | Lecture       | L2 (L01-1)              | Digital Logic, pt.1                                            |
|            | Fri 	Feb 09         | 11:00-12:45 | Lecture       | L3 (L01-2)              | Digital Logic, pt.2                                            |
| 7          | Mon 	Feb 13         | 15:30-17:15 | Lecture       | L4 (L02-1)              | Data Representation, pt.1                                      |
|            | Tue          Feb 14 | 11:00-12:45 | Tutorial      | T1                      | Digital Logic                                                  |
|            | Fri          Feb 17 | 11:00-12:45 | Lecture       | L5 (L02-2)              | Data Representation, pt.2                                      |
| 8          | Mon 	Feb 20         | 15:30-17:15 | Lecture       | L6 (L03)                | Introduction to Assembly                                       |
|            | Tue 	Feb 21         | 11:00-12:45 | Tutorial      | T2                      | Data Representation                                            |
|            | Tue 	Feb 21         | 15:30-19:15 | Lab           | P1                      | The Tuesday Lab                                                |
|            | Wed 	Feb 22         | 09:00-12:45 | Lab           | P1                      | The Wednesday Lab                                              |
|            | Fri 	Feb 24         | 11:00-12:45 | Lecture       | L7 (L04)                | ISAs                                                           |
|            | Fri 	Feb 24         | 13:30-17:15 | Lab           | P1                      | The Friday Lab                                                 |
| 9          | Mon 	Feb 27         | 15:30-17:15 | Lecture       | L8 (L05-1)              | Basic Processing Unit, pt.1                                    |
|            | Mon 	Feb 27         | 18:45-19:45 | **Exam**          | E1                      | Mid-term **Exam** on L1-7                                          |
|            | Tue 	Feb 28         | 11:00-12:45 | Tutorial      | T3                      | Discuss midterm                                                |
|            | Tue 	Feb 28         | 15:30-19:15 | Lab           | P2                      | The Tuesday Lab                                                |
|            | Wed 	Mar 01         | 09:00-12:45 | Lab           | P2                      | The Wednesday Lab                                              |
|            | Fri 	Mar 03         | 11:00-12:45 | Lecture       | L9 (L05-2)              | Basic Processing Unit, pt.2                                    |
|            | Fri 	Mar 03         | 13:30-17:15 | Lab           | P2                      | The Friday Lab                                                 |
|            | Fri 	Mar 03         | Anytime     | Self-study    | S1                      | Deadline Self-Study Part 1                                     |
| 10         | Mon 	Mar 06         | 15:30-17:15 | Lecture       | L10 (L06)               | I/O Subsystem                                                  |
|            | Tue 	Mar 07         | 11:00-12:45 | Tutorial      | T4                      | Basic Processing Unit                                          |
|            | Tue 	Mar 07         | 15:30-19:15 | Lab           | P3                      | Deadline assignment 1+2 for Tuesday Lab                        |
|            | Wed 	Mar 08         | 09:00-12:45 | Lab           | P3                      | Deadline assignment 1+2 for Wednesday Lab                      |
|            | Fri 	Mar 10         | 11:00-12:45 | Lecture       | L11 (L07)               | Memory Subsystem                                               |
|            | Fri 	Mar 10         | 13:30-17:15 | Lab           | P3                      | Deadline assignment 1+2 for Friday Lab                         |
| 11         | Mon 	Mar 13         | 15:30-17:15 | Lecture       | L12 (L08-01)            | Pipelining, pt. 1                                              |
|            | Tue 	Mar 14         | 11:00-12:45 | Tutorial      | T5                      | Memory and I/O Subsystems                                      |
|            | Tue 	Mar 14         | 15:30-19:15 | Lab           | P4                      | Deadline assignment 3+4 for Tuesday Lab                        |
|            | Wed 	Mar 15         | 09:00-12:45 | Lab           | P4                      | Deadline assignment 3+4 for Wednesday Lab                      |
|            | Fri 	Mar 17         | 11:00-12:45 | Lecture       | L13 (L08-02)            | Pipelining pt. 2                                               |
|            | Fri 	Mar 17         | 13:30-17:15 | Lab           | P4                      | Deadline assignment 3+4 for Friday Lab                         |
| 12         | Mon 	Mar 20         | 15:30-17:15 | Lecture       | L14 (L09)               | Advanced Digital Computer Systems: Parallel & Distributed      |
|            | Tue 	Mar 21         | 11:00-12:45 | Tutorial      | T6                      | Pipeling/PDS                                                   |
|            | Tue 	Mar 21         | 15:30-19:15 | Lab           | P5                      | Deadline extra assignments for Tuesday Lab                     |
|            | Wed 	Mar 22         | 09:00-12:45 | Lab           | P5                      | Deadline extra assignments for Wednesday Lab                   |
|            | Fri 	Mar 24         | 11:00-12:45 | Final Session | L15                     | Demos + Recap of the course + Fill in the knowledge graph quiz |
|            | Fri 	Mar 24         | 13:30-17:15 | Lab           | P5                      | Deadline extra assignments for Friday Lab                      |
|            | Fri 	Mar 24         | Anytime     | Self-study    | S2                      | Deadline Self-Study Part 2                                     |
| 13         | Wed 	Mar 29         | 18:45-21:00 | **Exam**          | E2                      | Final **Exam** on L8-14                                            |

---
References:
---
Tags: 
Created: 2023-03-28 17:27:22
---
(Links:: [[Parallel Processing and Performance]])
A context switch is a process of saving and restoring the state/context of a process or thread so that it can continue its execution from where it left off. Here are some key points:

- A context switch is initiated by the operating system's scheduler when it decides to switch from one process/thread to another.
- The saved state includes the program counter (PC), register values, and memory management information.
- When a context switch occurs, the operating system saves the state of the current process/thread into memory and restores the saved state of the next process/thread to be executed.
- Context switching is an expensive operation in terms of time and system resources, so it is generally avoided when possible.
- Multiprocessing/multithreading can increase the frequency of context switches, as more processes/threads are competing for CPU time.
- The use of hardware support, such as multiple processor cores, can reduce the overhead of context switching.
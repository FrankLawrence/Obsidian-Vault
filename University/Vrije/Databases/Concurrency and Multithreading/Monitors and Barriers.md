---
Tags: 
Created: 2024-12-18 04:27:21
---
(Links:: [[Concurrency and Multithreading]])
# Monitor
- In Java, a *monitor* is associated with an object.  
- It combines *data*, *methods* and *synchronization* in one modular package.
- Always at most one thread may be executing a method of the monitor.

A monitor provides mechanisms for threads to:  
- give up (exclusive) access until some condition is met; or 
- signal to other threads that such a condition holds.

- In Java, a **Condition object** associated with a lock allows a thread to *release* the lock temporarily, by calling the `await()` method.
- A thread is *woken up* when another thread performs the corresponding `signal()` or `signalAll()`.
# Bounded queue with locks and conditions
# Lost-wakeup problem
# Readers-writers Lock, Reentrant Lock, Semaphore
# Synchronized Method
# Sense-reversing Barrier
# Combining tree and tournament barriers
## Combining Tree
- Memory contention is reduced by spreading spinning on sense fields over different nodes.
- This is especially favourable for cacheless architectures.
- In cache-coherent architectures, sense fields of nodes should be kept at different cache lines, to avoid false sharing.
## Tournament Barrier
- The tournament barrier uses a binary tree of depth d. This a barrier for (at most) $2^{d+1}$ threads.
- Each node is divided into two parts: active and passive.  
- Both parts of the node have a (volatile) sense field, initially false. The active and passive part of a non-leaf have one child each.
- To each part of a leaf, one thread is assigned. Threads have a local sense, initially true.
- Threads spin on a local field, so there is no memory contention.
- Except possibly false sharing.
- Since there is no shared counter, no read-modify-write operation is needed to decrease it.
# Dissemination Barrier


---
References:
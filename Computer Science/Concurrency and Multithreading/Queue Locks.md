---
Tags: 
Created: 2024-12-18 01:25:43
---
(Links:: [[Concurrency and Multithreading]])
The [[Mutual Exclusion#Filter Lock|filter lock]] and the [[Mutual Exclusion#Bakery algorithm|bakery algorithm]] do not scale. Because they require at least n atomic registers for n threads.

Since hardware allows out-of-order execution, care is needed to place **memory barriers** or declare atomic registers **volatile**.

Read-modify-write operations are about as expensive as memory barriers and volatile variables.
When you try to acquire a lock you can do two things: keep trying by *spinning* (short duration) or give up the processor and retry later called *exponential backoff* (long durations).
# TAS and TTAS locks
Recall that `getAndSet()` sets an *AtomicBoolean* variable to true, and returns the previous value of this variable, in one atomic step.

The **TAS lock**:  
- The `AtomicBoolean` variable originally contains false.
- `lock()` repeatedly applies `getAndSet()` to the variable. The lock is obtained if false is returned.
- `unlock()` writes false to the variable.

Recall that *read-modify-write* operations include a memory barrier, invalidating cache lines.
The **TTAS lock**:
- The AtomicBoolean variable originally contains false.
- `lock()` spins on a cached copy of the variable. 
  When false is returned, apply `getAndSet()` to the variable. 
  The lock is obtained if false is returned. 
  Else go back to spinning on the cached variable.
- `unlock()` writes false to the variable.

The performance of the TAS lock is very bad.  
The performance of the TTAS lock is pretty bad in case of high contention.
To grasp this poor performance, we must dive into **cache coherence**:
- The shared bus is claimed by one broadcaster at a time. Processors and memory “snoop” on the bus.
- Random access memory is slow (tens of machine cycles). Caches are fast (one or two machine cycles).

Changes in a cache are accumulated, and written back when needed, to make place in the cache, at a memory barrier, or if another processor wants them.

**Cache coherence**: When a processor writes a value in its cache, all copies of this variable in other caches must be *invalidated*.

When a processor takes a *cache miss*, the required data is provided by main memory, or by a snooping processor.
## Why the TAS lock performs poorly
A `getAndSet()` call on the Boolean variable invalidates cache lines at all processors. As a result, all spinners take a *cache miss*, and go to the bus to fetch the (mostly unchanged) value of the variable.

So the spinners produce a *continuous storm* of unnecessary messages over the bus. To make matters worse, this delays the thread holding the lock.
## TTAS lock performance
When the lock is released, false is written to the Boolean variable, invalidating all cached copies. All spinners take a cache miss, and go to the bus to fetch the value of the variable.

Then they concurrently call `getAndSet()` to acquire the lock.   These calls invalidate the cached copies of the variable at other threads, leading to another round of cache misses.

Then the storm lies down, and threads return to local spinning on the cached variable.
## Exponential back-off TTAS
*Improvement* of the TTAS lock:  
- If the lock was free but I fail to get it, back off, to avoid collisions, because there is contention.  
- Each failure to get the lock doubles the max waiting time
- Each acquirement of the lock halves the max waiting time
- Randomly pick a duration between `min` and `max` to prevent lock-step (all coming back at the same time)

**Performance**:
- excellent performance in case of low contention

> [!warning] Drawbacks
> - All threads still spin on the same variable, causing cache coherence traffic when the lock is released
> - It is not *starvation-free*
> - Exponential backoff may delay threads longer than necessary, causing underutilization of the critical section
> - Its performance is very sensitive to `minDelay` and `maxDelay`. Optimal values are platform- and application-dependent
# Anderson array locks
- A (volatile) Boolean **array** represents the threads that are waiting for the lock.
- The array’s size *n* is the (maximum) number of (concurrent) threads. Initially slot $0$ holds true, while slots $1, . . . , n − 1$ hold false.  
- The **counter**, an `AtomicInteger`, initially is 0.  
- To acquire the lock, a thread applies `getAndIncrement()` to the counter. The returned value *modulo n* is its slot in the array. A thread waiting for the lock keeps spinning on (a cached copy of) its slot in the array, until it is *true*.
- To unlock, a thread first sets its slot in the array to false, and then the next slot (modulo n) to true.
- Each waiting thread spins on (a cached copy of) a different slot in the array, so releasing a lock gives **no cache coherence overhead**.

- **One read-modify-write operation** per lock access.  
- **Short hand-over time** compared to exponential backoff. 
- **Scales well** to large numbers of threads.  
- Provides (first-come-first-served) **fairness**.

> [!warning] Drawbacks
> - Protecting $L$ different objects takes **$O(L\cdot n)$ space**.
> - Not straightforward to **stop waiting** for the lock
> - Vulnerable to [[False Sharing]], because array slots tend to be kept on the same cache line
# CLH and MCS queue locks
## Craig Landin Hagersten Lock
- Threads wait for the lock in a (virtual) queue of nodes.  
- **tail**, an `AtomicReference<QNode>`, points to the last added node. Initially it points to a dummy node containing false.
- A node’s (volatile) Boolean `locked` field is:  
	- *true* while its thread is waiting for or holds the lock;
	- *false* when it has released the lock.
- A thread that wants the lock creates a node `ν`, containing true.  
	- It applies **getAndSet(ν)** to `tail`, to make `ν` the tail of the queue and get the node of its predecessor.  
	- It spins on (a cached copy of) locked in its predecessor’s node until that field becomes false. Then it takes the lock.
- A thread that releases the lock, sets the locked field of its node to false.
### Performance
- The CLH lock has the same *good performance* as the [[#Anderson array locks]], also provides *fairness*, uses *less space*, and isn’t vulnerable to [[False Sharing]].
- Protecting $L$ different objects takes $O(L + n)$ space.  
- After releasing the lock, a thread can *reuse* the node of its predecessor for a future lock access.  
- But the CLH lock performs poorly in a *cacheless nonuniform memory access architecture*, where remote spinning is expensive.

> [!question]+ CLH doesn't perform well on NUMA architecture. How can we adapt it to let threads spin on a *local variable*?
> The solution is the [[#Mellor-Crummey Scott queue lock]]
## Mellor-Crummey Scott queue lock
- Threads again wait for the lock in an (explicit) queue of nodes. *tail* points to the last added node. Initially it is null.

- A node’s *locked* field is true while its thread is waiting for the lock. Initially it is false!

- A node’s **next** field points to the node of the successor of its thread in the queue. Initially it is null.

- A thread that wants the lock creates a node `ν`.
	- It applies **getAndSet(ν)** to tail, to make `ν` the tail of the queue and get the node of its predecessor.
	- If tail was null, the thread takes the lock immediately.
	- Else it sets the locked field of `ν` to true and the next field of its predecessor’s node to `ν`. Next it spins on the `locked` field of `ν` until it becomes false. Then the thread takes the lock.

> [!question] Why does `locked` change to false when its thread *releases* the CLH lock versus when its thread *can take* the MCS lock?
> In the CLH lock, `locked` signals to the successor in the queue, while in the MCS lock it signals to its own thread.

- A thread that releases the lock, checks the next field of its node `ν`. 
	- If it points to a node, the thread sets the locked field of that node to false.  
	- If the next field of `ν` is null, then the thread applies **compareAndSet(ν,null)** to tail.  
- If this call fails, another thread is trying to acquire the lock.
- Then the thread spins on the `next` field of `ν` until a node is returned.
	- Next it sets the `locked` field of that node to false.
### Performance
- The MCS lock has the same *good performance* and *space complexity* as the CLH lock.
- Its performance does not require caches. If the lock is free, tail is a null pointer. A thread can *reuse* its own node.
- The price to pay is a more involved unlock method.
# CLH lock with timeout
- With exponential backoff, a waiting thread can abandon its attempt to get the lock. But with queue locks this is not so easy.
- We extend the CLH lock with timeouts.
- Again, threads wait for the lock in a queue of nodes.
- *tail* points to the last added node. Initially it is null.

The **pred** field in a node of a thread A contains a *pointer*, either: 
- null, if A is waiting in the queue or is in its critical section; 
- to the node **AVAILABLE**, if A left its critical section; or  
- to the node of A’s predecessor, if A timed out.

A thread A that wants the lock creates a node `ν` containing null. 
- A applies **getAndSet(ν)** to `tail`, to make `ν` the tail of the queue and get the node of A’s predecessor.
- If there is no predecessor, A takes the lock immediately.
- Else A spins on (a cached copy of) the `pred` field in the node of A’s predecessor until it is not null.
  If it points to the node **AVAILABLE**, A takes the lock. 
  If it points to a *new predecessor node*, A continues to spin on that node’s `pred` field.

A thread that abandons its attempt to get the lock:

- sets its pred field to its predecessor’s node, signaling to its successor that it has a new predecessor.

A thread that releases the lock:

- applies **compareAndSet(ν, null)** to tail, with `ν` the node of the thread.
	- If this call succeeds, the queue has become empty. 
	- If this call does not succeed, there is a successor. 
	  Then the thread sets its pred field to **AVAILABLE**, signaling to its successor that it can take the lock.

---
References:
---
Tags: 
Created: 2024-11-17 03:03:35
---
(Links:: [[Concurrency and Multithreading]])
# Consensus
A fundamental problem in distributed computing is to guarantee continued service when processes crash.
> [!definition] Binary consensus
> Threads must agree on a decision 0 or 1 (while some of them may crash) 

Each thread randomly chooses an *input* value 0 or 1, and (if it does not crash) eventually *decides* for 0 or 1.
In a binary consensus protocol, each execution satisfies:
- Consensus: All threads eventually decide for the same value
- Validity: This value is some thread's input

We aim for a *wait-free* consensus protocol. Wait-free consensus is tricky because the thread that enforces the decision may crash immediately after this event.
## Bivalent and critical states
A state of consensus protocol is **bivalent** if it exhibits executions to decisions 0 and 1. Else it is **univalent**.
> [!info] Lemma
> Each wait-free consensus protocol has a bivalent *initial* state

A state is **critical** if:
- it is *bivalent*; and
- any move by a thread results in a **univalent** state.

> [!info] Lemma
> Every wait-free consensus protocol has a critical state.
> Else there would be an infinite execution visiting only bivalent states.

# No consensus with atomic registers
> [!info] Theorem
> Wait-free 2-thread consensus can't be solved by atomic registers.
> > [!info]- Proof
> > Suppose, toward a contradiction, that a solution does exist. Given threads $A$ and $B$. Consider a critical state $s$.  
> > Let a move from $A$ lead to decision 0, and a move from $B$ to decision 1.
> > 
> > - If $A$ does a *read*, then $B$ can still run solo to decision 1. Likewise, if $B$ does a *read*, $A$ can still run solo to decision 0.
> > 
> > - Let $A$ and $B$ do *writes* to different registers.
> >   $B$oth orders of these two moves lead to the same state.
> > 
> > - Let $A$ and $B$ do *writes* to the same register. 
> >   If $A$ does its write, then $B$ can still run solo to decision 1. Likewise, if $B$ does its write, $A$ can still run solo to decision 0.
> > 
> > All cases contradict the fact that $s$ is critical.
# 2-thread consensus with a queue
> [!info] Theorem
> Wait-free 2-thread consensus can be solved by a wait-free FIFP queue with a dequeue method.

Given two threads. The queue initially contains two items: WIN and LOSE
Each thread *first writes its value in a MRSW register*.
Then it dequeues an item from the queue.
- If it dequeues WIN, it decides for its own value.
- If it dequeues LOSE, it gets and decides for the value in the other thread's MRSW register.

> [!info] Corollary
> It is impossible to implement a **lock-free FIFO queue** with two dequeuers using only atomic registers.

It is also impossible to implement a lock-free stack, list or set using only atomic registers.
# No 3-thread consensus with a queue
> [!info] Theorem
> Wait-free 3-thread consensus cannot be solved by wait-free FIFO queues (with only the enqueue and dequeue methods).

Suppose, toward a contradiction, that a solution does exist. Given threads $A$, $B$ and $C$. Consider a critical state $s$.
Let a move from $A$ lead to decision 0, and a move from $B$ to decision 1.
The following cases all contradict the fact that $s$ is critical.
> Let $A$ and $B$ perform moves on different queues. Both orders of these two moves lead to the same state.

In the following cases, two different move orders by $A$ and $B$ lead to the same queue structure, so $C$ cannot determine who moved first.
> Let $A$ and $B$ perform **dequeues** on the same queue (and crash). $C$ cannot (on its own) distinguish in which order they were done.
> If the queue is *nonempty*, these two moves lead to the same state, because they operate on different ends of the queue.
> If the queue is *empty*, $C$ cannot distinguish the dequeue of $B$ followed by the enqueue of $A$, from only the enqueue of $A$
  
In the final case, it can be seen from the queue who moved first. But to determine this, $A$ and $B$ must investigate on the queue whether $a$ or $b$ was enqueued first, thus restoring the original queue structure.
> Let $A$ **enqueue** $a$ and $B$ **enqueue** $b$ on the same queue.
> Let $A$ run solo until it *dequeues* $a$ or $b$ (and crash)
> This must happen before $A$ can decide between 0 or 1, to determine whether $B$ enqueued first.
> Next let $B$ run solo until it *dequeues* $b$ or $a$ (and crash).
> Now $C$ cannot distinguish whether $a$ or $b$ was enqueued first.
# No 3-thread consensus with commuting/overwriting operators
## Read-modify-write operations
At the hardware level, before a *read* or *write* operation is performed, first the bus (between processors and main memory) must be *locked*.
A **read-modify-write** operation allows a read followed by a write, while in the meantime the lock on the bus is kept.
The written value is determined using the value returned by the read.
It is advisable to use read-modify-write operations sparingly:
- They take significantly more clock cycles to complete than an atomic register.
- They include a memory barrier, invalidating cache lines, and prevent out-of-order execution (and other compiler optimizations)

> [!info] Why read-modify-write ops are lock-free
> Read-modify-write operations grab a hardware lock
> Software threads crash as a consequence of a hardware instruction
> This means threads cannot crash during a read-modify-write instruction and keep the lock on the bus.

Some standard read-modify-write operations on **AtomicInteger** in Java:
- `getAndSet(v)`: Assign `v`, and return the prior value.
- `getAndIncrement()`: Add 1, and return the prior value.
- `get()`: Return the value of the register.
- `compareAndSet(e,u)`: If the prior value is $e$, then assign $u$ as new value, else leave the value unchanged;
  Return a Boolean to indicate whether the value changed.

`getAndSet()` writes true in an **AtomicBoolean** register and returns the prior value.

> [!question] How can wait-free 2-thread consensus be solved with `getAndSet()`?
> Let a MRMW register contain false.
> Each thread first writes its value in a MRSW register.
> Then it applies `getAndSet()` to the MRMW register:
> - If it returns false, it decides for its own value
> - If it returns true, it get and decides for the value in the other thread's MRSW register.
## Commuting/overwriting read-modify-write operations
$f(v)$ denotes which value results after applying operator $f$ to a register with value $v$
> [!info] Theorem
> Let $F$ be a set of functions such that for all $f,g\in F$ and values $v$, **$f(g(v))=g(f(v))$** or **$f(g(v))=f(v)$** or **$g(f(v))=g(v)$**
> 
> 3-thread consensus cannot be solved by read-modify-write operations using only functions in $F$:
> > [!info]- Proof
> > Suppose a wait-free 3-thread consensus protocol does exist.
> > Given threads $A$, $B$ and $C$. Consider a critical state $s$.
> > Let a move from $A$ lead to decision 0, and a move from $B$ to decision 1.
> > If $A$ and $B$ apply read-modify-write moves to different registers, then both orders of these two moves lead to the same state.
> > Let $A$ and $B$ apply $f_{A}$ and $f_{B}$ to the same register (and crash).
> > - If **$f_{A}(f_{B}(v))=f_{B}(f_{A}(v))$**, then $C$ cannot distinguish in which order these moves were performed.
> > - If **$f_{A}(f_{B}(v))=f_{A}(v)$**, then $C$ cannot distinguish whether first $B$ and then $A$, or only $A$ moved.
> > - Likewise if **$f_{B}(f_{A}(v))=f_{B}(v)$**.
> > 
> > All cases contradict the fact that $s$ is critical.
## `compareAndSet` is not commuting or overwriting
Consider a register with the value 0. Let $k,l>0$ be distinct.
- First `compareAndSet(0,k)` and then `compareAndSet(0,l)` yields $k$.
  First `compareAndSet(0,l)` and then `compareAndSet(0,k)` yields $l$.
- First `compareAndSet(0,k)` and then `compareAndSet(0,l)` yields $k$.
  Only `compareAndSet(0,l)` yields $l$.
# Consensus with `compareAndSet`
## n-thread consensus with `compareAndSet`
> [!info] Theorem
> Wait-free $n$-thread consensus can be solved by `compareAndSet`, for any $n$.
> > [!info]- Proof
> > ```java
> > private AtomicInteger r = new AtomicInteger(-1);
> > public Object decide(Object value) {
> >   int i = ThreadID.get()
> >   proposed[i] = value
> >   if r.compareAndSet(-1, i)
> >      return proposed[i]
> >   else
> >      return proposed[r.get()]
> > }
> > ```

The wait-free consensus protocol for any number of threads can be used to obtain a wait-free implementation of each object.
# A universal construction
## Lock-free universal construction
The successive method calls that have been applied to the object are placed in an (unbounded) linked list.
A thread that wants to apply a method to the object, creates a node `ν` holding this method call.
The thread repeatedly tries to let the head of the list point to `ν`, by participating in a consensus protocol.

### Head array
- Finding the head of the list by traversing the entire list is clumsy
  -> **Solution**: Given `n` threads with IDs $0,...,n-1$. The *array* head contains `n` MRSW pointers to nodes in the linked list.
- `head[i]` points to the last node in the list that thread `i` observed
- Initially they all point to a sentinel node `tail` with sequence number 1
### Nodes
Each node in the *linked list* contains:
- a method call
- a sequence number
- a consensus object
- a pointer to the next node in the list

---
Suppose thread `i` wants to apply a method to the object. It creates a node $v$ holding this method call, sequence number 0, a consensus object, and pointer null.
Thread `i` repeatedly (until it wins) does the following:
- Traverse the array `head` to determine the node $v'$ with the highest sequence number $m$.
- Take part in the consensus object of $v'$

If thread `i` wins, it:
- lets $v'$ point to $v$,
- sets the sequence number of $v$ to $m+1$, and
- lets `head[i]` point to $v$

> [!example]

If thread `i` finds that another thread `j` won on the consensus object at the head $v'$ of the list, then `i`:
- finds the node $v''$ of `j` (via the consensus object);
- directs $v'$ to $v''$
- writes the correct sequence number in $v''$; and
- directs `head[i]` to $v''$

Else the algorithm would not be lock-free:
A node that wins on the consensus object and then crashes would halt all other threads.

> [!info]- Correctness
> The construction is **lock-free**:  
> - As long as there are method calls, such calls keep on being added
> to the head of the list.
> - Competing threads help to advance the pointer from the old to the new head, and update the sequence number of the new head. as well as the head array.
> 
> The *linearization point* of a method call is the moment it wins on a consensus object.
> 
> The construction is **not wait-free**:
> A thread may infinitely often fail to add its method call at the head of the list.

## A wait-free universal construction
When a thread `i` wants to add a method call to the list, it:
1. *lets `announce[i]` point to its node, holding this method call*, and
2. traverses the array `head` to set `head[i]` to the head of the list.

Then it repeatedly competes for the consensus object at `head[i]`. If `i` loses, it helps the winning thread advance the head of the list.
If `i` wins, it checks if `announce[k]`, with `k` the next sequence number `modulo n`, contains a pending method call (i.e. has seq. nr. 0).
Let $v$ denote either `announce[k]` if it contains a pending method call, or else `i`'s node.
- `i` makes the consensus object point to $v$,
- lets the old head point to $v$,
- sets $v$'s sequence number to the sequence number of the old head plus one, and
- lets `head[i]` point to $v$

Thread `i` may continue to help (at most $n-1$) other threads, until its own method call is guaranteed to be appended to the list.

> [!question] Suppose thread `i`'s node was added by another thread `j` who won on the consensus object. How will `i` see this is the case?
> Before retrying on another consensus object, `i` first checks whether its node still carries sequence number 0.

Care is needed to avoid that:  
- `i` sees this sequence number is 0,  
- another thread adds `i`’s node to the list, and 
- `i` wins on a consensus object.

To prevent this scenario, `i` sets `head[i]` to `max(head)`, from that point tries all consecutive consensus objects, and each time first checks the sequence number in its node.

> [!info]- Correctness
> The construction is **wait-free**: If a thread i has a pending method call, and the next sequence number of the list equals i modulo n, then this method call is certain to be added to the head of the list.
> 
> The *linearization point* of a method call is the moment its node is appended to the list.

> [!summary] Practical relevance of the theoretical results
> Linearization is a corner stone for reasoning about multicore programs.
> 
> At the physical level, only safeness of a register is guaranteed. Upgrading this to an atomic register requires a “software” solution.
> 
> Basically, each multicore program is solving a consensus problem. So the impossibility results show the importance of compareAndSet.
> 
> The universal construction shows the power of consensus. With it, a wait-free implementation is possible of any object.
> 
> The universal construction underlines the importance of helping threads in lock- and wait-free implementations.

---
References:
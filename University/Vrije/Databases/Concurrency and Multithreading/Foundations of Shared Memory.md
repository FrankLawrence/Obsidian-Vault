---
Tags: 
Created: 2024-11-04 21:26:28
---
(Links:: [[Concurrency and Multithreading]])
# Linearizability and Sequential consistency
An object has a state (values of variables) and a set of *methods*.
- *if* (precondition)
	- the object is in such-and-such a state before the method is called
- *then* (postcondition)
	- the method call will return a particular value
	- or throw a particular exception
- *and* (postcondition, continued)
	- the object will be in some other state when the method call returns

> [!example] Pre and postconditions
> Consider a dequeue method on a FIFO queue
> *precondition*: the queue is nonempty
> *postcondition*: returns the head of the queue
> *postcondition*: removes the head from the queue
> 
> *precondition*: the queue is empty
> *postcondition*: throws `EmptyException`
> *postcondition*: the queue remains unchanged

## Why sequential object totally rock
An object state is meaningful between method calls. Intermediate states of the object while a method call is in progress can be ignored. Interactions among methods depend only on their effect on the object state.
Each method can be considered in isolation. New methods can be added without changing the description of old methods.
- The order in which a single thread issues method calls is called its *program order*
- Method calls should appear to happen in a one-at-a-time sequential order
- Method calls should appear to take effect in program order

## Linearizability
We order the method calls in an execution, by contracting each of them to a single moment in time, when the method call is active. Then we can reason about the execution as if it were single-threaded.
An *execution* on a concurrent object is **linearizable** if:
- each method call can be condensed to one a moment in time, its linearization point, between its invocation and return events, 
- in line with the system specification

An *object* is **linearizable** if all its possible executions are linearizable.
> [!example]
> - Consider a FIFO queue `q`.
> - Since `x` is enqueued before `y`, it should also be dequeued before `y`.
> ![[Linearizability Diagram.svg|400]]

For a method call that has an invocation but no return event, one can either:
- let it take effect before the end of the execution; or
- omit the method call from the execution altogether, 
  i.e. it did not take effect before the end of the execution

Consider a concurrent *bounded* FIFO queue `q`, with an enqueue (`q.enq(x)`) and a dequeue (`q.deq()`) method. There are two threads: An enqueuer and a dequeuer. (Conflicts between the two threads can be avoided by placing the queue under a lock. -> but yields inefficient implementation [[Parallel Processing and Performance#^3d1eae|Amdahl's law]])
## Sequential consistency
For multiprocessor memory architectures, linearizability is often considered too strict.
An *execution* on a concurrent object is **sequentially consistent** if each method call in the execution:
- appears to take effect instantaneously
- is in program order on each thread (the order in which a single thread issues method calls)
- is in line with the system specification

An *object* is **sequentially consistent** if all its possible executions are sequentially consistent. Sequential consistency is less restrictive than linearizability, because it allows method calls to take effect after they returned.
![[43894.png|500]]
![[49939.png|500]]
## Compositionality
Linearizability is compositional: The composition of linearizable objects is again linearizable.
By contrast, sequential consistency is not compositional.
![[Compositionality.svg|400]]
The executions for objects p and q are by themselves sequentially consistent, but their composition is not.
Which correctness requirement is right for a given application?
This depends on the needs of the application
- A printer server can allow jobs to be reordered
- A banking server better be sequentially consistent.
- A stock-trading server requires linearizability

We will stick to linearizability, as it is well-suited for high-level objects.
Most hardware architectures do not support sequential consistency, as it outlaws compiler optimizations such as out-of-order execution.

# Wait-free bounded queue for two threads
Parameter *capacity* contains the bound on the size of the FIFO queue.
The queue is implemented as an array of size `capacity`.
- `head` points at the `head` of the queue
- `tail` points at the first empty slot

The values of `head` and `tail` are interpreted modulo capacity. The queue is empty if `head = tail` and full if `tail - head = capacity`.

```java
class WaitFreeQueue⟨T⟩ {  
	volatile int head = 0, tail = 0; 
	volatile T[] items;  
	public WaitFreeQueue(int capacity) {
		items = T[] new Object[capacity]; 
	}
	public void enq(T x) throws FullException {  
		if tail - head == capacity throw new FullException(); 
		items[tail % capacity] = x; 
		tail++;
	}  
	public T deq() throws EmptyException {
	    if tail == head throw new EmptyException();
	    T x = items[head % capacity];
	    head++;
	    return x;
	} 
}
```
> [!info]- Wait-free bounded FIFO queue: Correctness
> Intuitively, this algorithm is correct for the following reasons:
> - Only the *enqueuer* writes to `tail` and `items[...]`, and only the *dequeuer* writes to `head`
> - The condition `if tail - head == capacity` in `enq()` stops the *enqueuer* from overwriting an element in the queue before the *dequeuer* has read it.
>   Here it is used that the dequeuer only increases `head` after it read `items[head % capacity]`.
> - The condition `if tail == head` in `deq()` stops the *dequeuer* from reading an element in the queue before the *enqueuer* has placed it in the queue.
>   Here it is used that the enqueuer only increases `tail` after it wrote to `items[tail % capacity]`.

> [!info]- Linearizability
> - The linearization point of a successful `enq()` is at (writing to tail in) `tail++`.
> - The linearization point of an unsuccessful `enq()` is at (reading the `head` value to perform) `if tail - head == capacity`.
> - The linearization point of a successful `deq()` is at (writing to head in ) `head++`.
> - The linearization point of an unsuccessful `deq()` is at (reading the `tail` value to perform) `if tail == head`.

> [!question]- Why is it too late to linearize an unsuccessful `enq()` at `throw new FullException()`?
> The dequeuer might be called before, changing the value of head -> enqueuer says it's full after an element has been dequeued

> [!question]- Why is it too late to linearize an unsuccessful `enq()` when `if tail - head == capacity` returns `true`?
> 
## Flawed bounded FIFO queue
In the dequeuer, let's swap the order of two program statements: 
1. `head++;`
2. `T x = itmes[(head - 1) % capacity];`

Let `capacity` be 1.
Suppose the enqueuer applies `enq(a)` and `enq(b)` to an empty queue, and the dequeuer concurrently applies `deq()`.
Let `head` and `tail` be 0. The following execution is not linearizable:
- enq: `tail - head == 0`
- enq: `items[0] = a`
- enq: `tail = 1`
- deq: `tail - 1 == 1`
- deq: `head = 1`
- enq: `tail - head == 0`
- enq: `items[0] = b`
- deq: `x = b`
- deq: `return b`
## Safe, Regular and Atomic Registers
A *single-writer* register is **safe** if every `read()` that does not overlap with a `write()` returns the last written value.
A `read()` that overlaps with a `write()` may return any value. At hardware level, only safe **Boolean** registers are provided:
- The writer sets a voltage level either high or low
- Setting a level high when it is already high may case a temporary perturbation of the level.

So a reader might read 0, although the register's value is 1.
A *single-writer* register is **regular** if:
- it is safe; and
- every `read()` that overlaps with `write()'s` returns either one of the values written by these overlapping `write()'s` or the last written value before the `read()`

A register is **atomic** if it is linearizable to the sequential register (or a uniprocessor).
> [!example]
> ![[85970.png|500]]
> The first and second `read()` would have to be linearized after and before `write(2)`, respectively

# From Safe Boolean MRSW to Atomic multi-valued MRMW registers
From *safe* Boolean MRSW registers, we build *atomic* multi-valued MRMW registers, in a wait-free manner. As a first step, a *safe* Boolean MRSW register is turned into a *regular* Boolean MRSW register.
Before a write, the writer first reads the value in the safe register. If the written value is the same as the current value, then the write leaves the register untouched. Only if the written value is different from the current value, the write is performed physically.

> [!info] Correctness
> A read that does not overlap with a write returns the last written value, because the original register is safe.
> 
> Suppose a read overlaps with writes:
> - If all writes leave the value unchanged, no write is physically performed, so the read returns the last written value.
> - If a write changes the value, the read is allowed to return both a 0 and a 1.

## From regular Boolean MRSW to regular M-valued MRSW
AN M-valued MRSW register can contain values $0,1,...,M-1$
It is represented by M regular Boolean MRSW registers. The value of the M-valued register is *$k$* if Boolean registers $0,...,k-1$ contain 0 while Boolean register $k$ contains 1.
- *Write* value $k$: Write 1 in Boolean register $k$. Then write 0 in Boolean registers $k-1,...,0$ (in this order)
- *Read*: Read the Boolean registers from 0 up to $M-1$, until the value 1 is encountered. The index of this slot is returned.

> [!info] Correctness
> Let *$k$* be the last written value in the M-valued register before a certain read.
> Suppose this read returns a value $\neq k$.
> 1. The read **returns an $l< k$**.
>    Then the 1 in register $l$ was written by an overlapping write.
> 2. The read **encounters 0's in registers $0,...,k$**
>    Then an overlapping write wrote 1 in a register $l>k$, and next 0 in registers $l-1,...,k$
>    The read proceeds beyond register $k$ and ultimately returns the value of an overlapping write.

> [!question] What could go wrong if a write of value $k$ would first write 0 in register $k-1,...,0$ and then 1 in register $k$?
> A read could then encounter M 0's

> [!question] What could go wrong if a write of value $k$ would first write 1 in register $k$ and then 0 in registers 0,...,k-1 (in this order)?
> A read could then return an obsolete value between the last written value and $k$.
 > > [!example]
 > > Let the registers contain 1010. The value 3 is written. The writer writes 1 in register 3, and next 0 in register 0. A concurrent reader could now return the obsolete value 2.

## From regular SRSW to atomic SRSW
The *writer* to the SRSW register provides each write with a *timestamp* that increases at each write (The initial value of the register carriers the timestamp 0).
The *reader* remembers the latest value/timestamp pair it ever read, i.e. the pair with the greatest timestamp.
If it reads a value with timestamp smaller than the previous read, the reader ignores that value, and uses the remembered last value.
> [!example]
> ![[16388.png|400]]
> `write(2)` can be linearized before the two reads.

> [!info] Properties of regular and atomic registers
> Given an execution of reads and writes on a register.
> Let $W^i$ be the $i$th write, according to some linearization order, and $R^i$ a read of the corresponding value.
> $W^0$ "writes" the initial value at the start; $R^0$'s read this value.
> A register is **regular** if:
> - never $R^{i}\to W^{i}$; and
> - never $W^{i}\to W^{j} \to R^{i}$
> 
> A register is **atomic** if moreover $R^{i}\to R^{j}$ implies $i\leq j$.

> [!info] Correctness
> Since the original register is regular, clearly never $R^{i}\to W^{i}$ or $W^{i}\to W^{j} \to R^{i}$
> Owing to the timestamps, and the fact there is only one reader, a read never returns an older value than earlier reads.
> That is, if $R^{i}\to R^{j}$, then $i\leq j$.

> [!question]- Give an example to show that this timestamp approach is incorrect in case of *multiple* readers
> Let the regular register contain the pair $u/t$.
> The writer is writing $v/t+1$ while two consecutive reads are performed by different threads.
> - $A_{1}$ reads and returns $v/t+1$
> - $A_{2}$ reads and returns $u/t$

## From atomic SRSW to atomic MRSW
Given $n$ readers and one write. First we discuss an *incorrect* attempt.
The MRSW register consists of $n$ atomic SRSW registers, one for each *reader*.
The *writer* writes a value to each SRSW register (one at a time).
The following scenario shows that this MRSW register is not atomic:
1. The writer starts writing to the MRSW register by writing a new value to the SRSW register of reader $A$.
2. $A$ reads the *new* value in its SRSW register, and returns
3. $B$ reads the *old* value in its SRSW register, and returns
4. The writer writes the new value to the SRSW register of $B$.

---

Given $n$ readers $A_{i}$ (for $i=0,...,n-1$) and one writer.
The MRSW register consists of $n\times n$ atomic SRSW registers `a_table[0..n-1][0..n-1]` with *timestamped* values.
- The *writer* can write to the register `a_table[i][i]`.
- Each $A_{i}$ can write to the registers `a_table[i][j]` for all $j\neq i$.

The **writer** writes a *value*/*timestamp* to `a_table[i][i]` for each $i$; the timestamp is increased at each write call.
Each **reader** $A_{i}$ at a read:
- reads `a_table[j][i]` for all $j$, and picks the value with the highest timestamp; and
- writes this value/timestamp to `a_table[i][j]` for all $j\neq i$.

> [!example]-
> |     | 0   | 1   |
> | --- | --- | --- |
> | 0   | t   | t   |
> | 1   | t   | t   |
> 
> $n=2$, and all slots in `a_table` carry value $u$ and timestamp $t$.
> A *write* starts, and writes a new value $v$ with timestamp $t+1$ to `a_table[0,0]`.
> A *read* by $A_{0}$ starts, reads `a_table[0][0]` and `a_table[1][0]`, and selects $v/t+1$.
> Next it writes $v/t+1$ to `a_table[0][1]`, and returns $v$.
> A *read* by $A_{1}$ starts, reads `a_table[0][1]` and `a_table[1][1]`, and selects $v/t+1$.
> ![[81488.png|400]]

> [!example]-
> |     | 0   | 1   |
> | --- | --- | --- |
> | 0   | t+1   | t   |
> | 1   | t   | t   |
> A *write* starts, and writes a new value $v$ with timestamp $t+1$ to `a_table[0,0]`.
> A *read* by $A_{0}$ starts, reads `a_table[0][0]` and `a_table[1][0]`, and selects $v/t+1$.
> A *read* by $A_{1}$ starts, reads `a_table[0][1]` and `a_table[1][1]`, and selects the *old* value $u$ with timestamp $t$.
> $A_{0}$ writes $v/t+1$ to `a_table[0][1]`, and returns $v$.
> ![[81315.png|400]]

> [!info] Correctness
> Clearly never $R^{i} \to W^{i}$.
> Each write call overwrites the diagonal of `a_table` by values with a higher timestamp. Read calls consider a pair on the diagonal, and preserve the diagonal.
> This guarantees that never $W^{i}\to W^{j}\to R^{i}$.
> 
> Suppose a read by $A_{k}$ completely precedes a read by $A_{l}$.
> Let the read by $A_{k}$ return a value $u$ with timestamp $t$.
> $A_{k}$ writes u/t to `a_table[k][l]` before the read by $A_{l}$ starts.
> $A_{l}$ reads `a_table[m][l]` for all $m$, so `a_table[k][l]` in particular.
> Therefore the read by $A_{l}$ returns a value with a timestamp $\geq t$.
> Hence, if $R^{j}\to R^{j}$, then $i\leq j$.

## From atomic MRSW to atomic MRMW
Given $n$ readers/writers $A_{i}$ (for $i=0,...,n-1$). Then MRMW register consists of $n$ atomic MRSW registers `a_table[0..n-1]` with timestamped values.
Each $A_{i}$, to *write* a value $u$:
- reads `a_table[j]` for all $j$;
- picks a timestamp $t$, higher than any it observed; and
- writes $u/t$ to `a_table[i]`

Each $A_{i}$, to *read*:
- reads `a_table[j]` for all $j$; and
- returns a value with the highest timestamp; if multiple registers carry this timestamp, it takes the one with the largest index.

> [!example]-
> | 0     | 1     | 2   | 3     |
> | ----- | ----- | --- | ----- |
> | w/t-2 | x/t+1 | y/t | z/t+1 |
> 
> - $A_{0}$ and $A_{1}$ start a *write* of value $v_{0}$ and $v_{1}$, respectively. They concurrently read the registers, and both pick timestamp $t+2$.
> - $A_{0}$ *writes* $v_{0}/t+2$ to `a_table[0]`.
> - $A_{2}$ starts a *read*. It reads the registers, and returns $v_{0}$
> - $A_{1}$ *writes* $v_{1}/t+2$ to `a_table[1]`.
> - $A_{3}$ starts a *read*. It reads the registers, and returns $v_{1}$
> 
> ![[38368.png|500]]

> [!example]-
> | 0     | 1     | 2   | 3     |
> | ----- | ----- | --- | ----- |
> | w/t-2 | x/t+1 | y/t | z/t+1 |
> 
> - $A_{0}$ and $A_{1}$ start a *write* of value $v_{0}$ and $v_{1}$, respectively. They concurrently read the registers, and both pick timestamp $t+2$.
> - $A_{1}$ *writes* $v_{1}/t+2$ to `a_table[1]`.
> - $A_{2}$ starts a *read*. It reads the registers, and returns $v_{1}$
> - $A_{0}$ *writes* $v_{0}/t+2$ to `a_table[0]`.
> - $A_{3}$ starts a *read*. It reads the registers, and returns $v_{1}$
> 
> ![[89568.png|500]]

> [!info] Correctness
> Clearly never $R^{i} \to W^{i}$.
> - Suppose a write by $A_{k}$ completely precedes a write by $A_{l}$.
> - $A_{k}$ writes its value/timestamp in `a_table[k]`.
> - So $A_{l}$ will pick a timestamp greater than the timestamp of $A_{k}$.
> - Hence subsequent reads will never return the write from $A_{k}$.
> 
> So never $W^{i}\to W^{j}\to R^{i}$.
> - Suppose a read by $A_{k}$ completely precedes a read by $A_{l}$.
> - Let the read by $A_{k}$ return a value from `a_table[i]` with timestamp $t$.
> - The read by $A_{l}$ reads in `a_table[i]` a value with a timestamp $\geq t$. So it will return a value with a timestamp $\geq t$.
> - And if $A_{l}$ returns a value with timestamp $t$, then this pair must originate from `a_table[j]` for some $j\geq i$.
> - We **linearize** writes *lexicographically* on timestamp and index.
> - So the write corresponding to the read by $A_k$ is linearized no later than the write corresponding to the read by $A_l$.
> 
> Hence, if $R^{i}\to R^{j}$, then $i\leq j$.

> [!question]
> Suppose reading is adapted as follows:
> > if multiple registers carry the largest timestamp, take the one with the *smallest* index.
> 
> How should we then adapt the argumentation for
> > if $R^{i}\to R^{j}$, then $i\leq j$?
> 
> A write by $A_k$ is linearized before a write by $A_{l}$ if
> - either it has a smaller timestamp,
> - or the same timestamp and $k>l$

---
References:
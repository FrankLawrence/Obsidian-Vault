---
Tags: 
Created: 2024-10-30 14:41:38
---
(Links:: [[Concurrency and Multithreading]])
# Critical sections  
- A thread exhibits a sequence of atomic events $a_{0},a_{1},a_{2},\dots$
  Ex: read or write to a variable
	- Events are *instantaneous* and *never simultaneous*
- Consider the events of an execution by a multicore system
	- $a\to b$ denotes that $a$ happens before $b$. This is *total order*
	- Let $a$ and $b$ be events by the same thread, with $a\to b$. **$(a,b)$** denotes the time interval between these events
	- We write **$(a,b)\to(a',b')$** if $b\to a'$ (*partial order*: reflexive, transitive, anti-symmetric)
- A **critical section** is a block of code that should be executed by at most one thread at a time
- Let *$CS$* and *$CS'$* be time intervals in which two different threads execute their critical sections. **Mutual exclusion**: For each such pair, *$CS\to CS'$* or *$CS'\to CS$*

## Lock interface in Java
```java
public interface Lock {
	public void lock();
	public void unlock();
}

mutex.lock()
try{
	//critical section
} finally {
	mutex.unlock();
	//unlocking instructions
}
```

- `finally` part will *always execute*, ensuring that the lock is release lock properly
- Variables under a lock tend to be important for synchronization between different threads. When a thread *acquires* a lock, its working memory is invalidated, to ensure that values of fields are reread from main memory. When a thread *release* a lock, values of modified fields in its working memory are written back to main memory.

> [!info] Deadlock-free
> If a thread calls `lock()` but never acquires the lock, then other threads must be completing an infinite number of critical sections. 

> [!info] Starvation-free
> If a thread calls `lock()` or `unlock()`, then it will eventually acquire or release the lock, respectively.

> [!question] Why is releasing of the lock included in the definition of deadlock- and starvation-freeness?
> Else the following scenario would be allowed.
> A thread holding the lock calls `unlock()`
> This call never completes
> No other thread ever tries to get the lock

- `i` and `j` are single-reader, single-writer (SRSW) *registers*
- `flag[0]` and `flag[1]` are multi-reader, single-writer (MRSW) registers
- `victim` is a multi-reader, multi-writer (MRMW) register

Given two threads, with IDs 0 and 1

```java
class LockOne implements Lock {
	private boolean[] flag = new boolean[2];
	public void lock() {
		int i = ThreadID.get();
		int j = 1 - i;
		flag[i] = true;
		while (flag[i]) {}    // wait until flag[j] == false
	}
	public void unlock() {
		int i = ThreadID.get();
		flag[i] = false;
	}
}
```

`LockOne` provides mutual exclusion, but may *deadlock* (if both threads concurrently set their flags to true).
# Peterson Lock
```java
class Peterson implements Lock {  
	private boolean[] flag = new boolean[2]; 
	private int victim;  

	public void lock() {
	    int a = ThreadID.get();
	    int b = 1 - a;
	    flag[a] = true;
	    victim = a;
		while (flag[b] && victim == a) {};  //wait until other thread has flag false or wrote to victim  
	}
	public void unlock() {
	    int a = ThreadID.get();
	    flag[a] = false;
	} 
}
```
> [!info]+ The Peterson lock provides mutual exclusion (for two threads).
> Let thread `a` enter its critical section: `flag[a] == true`
> There are two possibilities:
> 1. Before entering, `a` read `flag[b] == false`
>    To enter, `b` first sets (`flag[b] = true` and `victim = b`)
> 2. Before entering, `a` read `victim == b`
>    Then `b` performed `victim = b` after `a` performed `victim = a` and so after `a` performed `flag[a] = true`
> 
> In both cases, `b` can only enter after `a` sets `flag[a] = false` or `victim = a`. Hence `b` cannot enter while `a` is in its critical section.

> [!info]+ The Peterson lock is starvation-free
> Let thread `b` try to enter its critical section.
> Then it sets `flag[b] = true` and `victim = b`
> Since `b` can enter when `flag[a] = false`, `a` could only starve `b` by repeatedly re-entering its critical section
> However, before entering, `a` sets `victim = a`, then `b` can enter its critical section

## Binary Tree
- A way to generalize the Peterson lock to $n\geq 2$ threads is to use a binary tree, where each node holds a Peterson lock for two threads.
- To each leaf in the tree, two threads are assigned. Threads that want to enter their critical section start at their leaf, and move level up when they acquire the lock at a node. 
- A thread that holds the lock of the root can enter its critical section. When a thread exits its critical section, it releases the locks of nodes that it acquired, on the path from its leaf to the root.

# Filter Lock
- Generalization of Peterson lock to $n\geq 2$ threads
- There are $n-1$ "waiting rooms", called *levels*, from 0 up to $n-1$
- Threads start at level 0. The critical section is at level $n-1$
- At most $n-l$ threads can concurrently proceed to a level $\geq l$

![[27667.png|300]]

For levels $l\geq 1$, there is a variable `victim[l]`. A thread `i` at a level $l-1$ that wants to go to level $l$, first sets `level[i] = l` and then `victim[l] = i`. Thread `i` must wait with going to level $l$ until either `level[j] < l` for all $j\neq i$ or `victim[l] != i`.

That is, thread `i` spins on:
- `level[j]` for each $j\neq i$, to check whether they are all $< l$; and
- `victim[l]` to check whether it is unequal to `i`

Again, in Java the `level[_]` and `victim[_]` fields must be volatile
> [!example]-
> - Let $n = 3$. Threads `A`,`B`,`C` are all at level 0.
> - `B` sets `level[B] = 1` and `victim[1] = B`.
> - Since no other thread has a level $\geq 1$, thread `B` proceeds to level 1.
> - `C` sets `level[C] = 1` and `victim[1] = C`.
> - `A` sets `level[A] = 1` and `victim[1] = A`.
> - Since `victim[1] != C`, thread `C` proceeds to level 1.
> - `C` sets `level[C] = 2` and `victim[2] = C`
> - `B` sets `level[B] = 2` and `victim[2] = B`
> - Since `victim[2] != C`, thread `C` proceeds to its critical section.
> - Since `level[C] = 2` and `victim[2] = B`, thread `B` must wait.

> [!info]+ The Filter lock provides mutual exclusion
> For each level $l$, the following property is always satisfied: At most $n-l$ threads are at a level $\geq l$.
> Namely, for each $l$, either:
> 1. at most one thread is at a level $\geq l$;
> 2. or a thread is waiting at each level $0,...,l-1$
> 
> In both cases the claim holds. Taking $l = n-1$, only one thread can be in its critical section, at any moment in time.

> [!info]+ The Filter lock is starvation-free
> Namely, consier a thread `i` waiting to go to a level $l\geq 1$.
> If no other thread wants to be in its critical section, `i` can proceed unhindered. If other threads keep on entering and leaving their critical section, eventually a thread $j \neq i$ wants to enter level $l$ and sets `victim[l] = j`. Then thread `i` can proceed to level $l$.

The `lock()` method can be split into two parts:
- a **doorway** part (which completes in a finite number of steps)
- a **waiting** part (which may include spinning, i.e. repeatedly reading variables until certain values are read)

**Fairness**: If a thread `i` completes its doorway before another thread `j` starts its doorway, then `i` enters its critical section before `j`.

> [!danger] Filter lock is not fair!

# Volatile variables 
## Out-of-order execution
- A processor executes instructions ordered by the availability of *input data*, rather than by their order in the program
- Processors can execute thousands of (read) instructions, while writes in the cache are lazily written back to memory
- In multicore programming, one thread can spy on another and observe out-of-order execution
- Hence reads/writes for synchronization must be announced explicitly. These come with performance penalty
- This makes sense because the vast majority of instructions is not for synchronization
## Volatile variables in Java
In Java, a variable can be declared *volatile*.
- When a volatile variable is *read*, its value is fetched from main memory (instead of from the cache)
- When a volatile variable is *written*, the new value is immediately written back to main memory
- Reads and writes to other variables before a write to a volatile variable cannot be reordered after this write
- Reads and writes to other variables after a read to a volatile variable cannot be reordered before this read

In the Peterson lock, the two `flag` fields and `victim` must be declared volatile. Else threads could read stable `flag` and `victim` values, due to out-of-order execution.
# Bakery algorithm
- The bakery algorithm provides mutual exclusion and is fair (guarantees the *first-come-first-server* property)
- To enter its critical section, a thread sets a *flag*, and takes a *number* greater than the numbers of all other threads
- When all lower numbers have been served, the thread can enter. At least its critical section, the thread resets its `flag`
- *Complication*: Threads may concurrently take the same number
- *Solution*: Lexicographical order -> $(l,i) < (m,j)$ if either $l < m$, or $l = m$ and $i < j$
```java
class Bakery implements Lock { 
	boolean[] flag;  
	Label[] label;  
	public Bakery (int n) {
		flag = new boolean[n];  
		label = new Label[n];  
		for (int k = 0; k < n; k++) {
			flag[k] = false; 
			label[k] = 0; 
		} 
	}
	
	public void lock() { 
		int i = ThreadID.get();  
		flag[i] = true;  
		label[i] = max(label[0],...,label[n-1]) + 1;  
		while ∃k (flag[k] && (label[k],k) < (label[i],i)) {};
	}  
	public void unlock() { 
		int i = ThreadID.get();
		flag[i] = false; 
	}
}
```

> [!example]-
> flag[1] = true  
> flag[0] = true  
> $A_0$ and $A_1$ read the label values  
> label[0] = 1  
> $A_0$ reads flag[1] == true and (label[1],1) < (label[0],0) 
> label[1] = 1  
> $A_1$ reads flag[0] == true and (label[0],0) < (label[1],1) 
> $A_0$ reads (label[0],0) < (label[1],1)  
> $A_0$ enters its critical section  
> $A_0$ exits its critical section  
> flag[0] = false  
> flag[0] = true  
> $A_0$ reads the label values  
> label[0] = 2  
> $A_0$ reads flag[1] == true and (label[1],1) < (label[0],0) 
> $A_1$ reads (label[1],1) < (label[0],0)
> $A_1$ enters its critical section

> [!info]+ Bakery has Mutual exclusion
> - Suppose, toward a contradiction, that two threads `i` and `j` are concurrently in their critical sections.
> - Let `(lavel[i],i) < (label[j],j)`
> - When `j` successfully completed the test in its waiting section, it read either `flag[i] == false` or `(label[j],j) < (lavel[i],i)`.
> - Since label values of threads (in particular, of `i`) only increase over time, `j` must have read `flag[i] == false`.
> - So before entering its critical section, `i` must have selected a label greater that `label[j]`. This contradicts `(label[i],i) < (label[j],j)`.

The *doorway* of a thread consists of setting its `flag` and computing its new `label`. If thread `j` starts its doorway after thread  `i` has completed it, then `j` will select a `label` greater than `label[i]`. 
Since `flag[i] == true`, `i` will enter its critical section before `j`.

> [!question]+ Why does bakery algorithm not scale to large dynamic systems
> 1. Labels may become arbitrarily large. But this can be circumvented (using modulo arithmetic)
> 2. The number of threads must be fixed beforehand
> 3. For $n$ threads, it requires reading $\geq n$ distinct variables. With only *read/write registers*, this cannot be avoided!

> [!question]+ Why is the mutual exclusion algorithm below for two threads flawed?
> - A MRMW register initially has the value -1
> - When thread $A_{0}$ (or $A_{1}$) wants to enter its critical section, it spins on the register until it is -1.
> - Then $A_{0}$ (or $A_{1}$) writes the value 0 (or 1) into the register
> - Next $A_{0}$ (or $A_{1}$) checks whether the value of the register is 0 (or 1)
> - If not, it returns to spinning on the register until it is -1. If so, it enters its critical section.
> - When a thread exits its critical section, it writes -1 into the register.
> 
> > [!info]- Solution
> > 1. Both read -1
> > 2. $A_{0}$ writes 0 into the register
> > 3. $A_{1}$ becomes slow
> > 4. $A_{0}$ reads 0 from register and enters CS
> > 5. $A_{1}$ writes 1 into register
> > 4. $A_{1}$ reads 1 from register and enters CS!


# Lower bound on number of locations 
> [!info] Theorem
> At least $n$ read/write registers are needed to solve deadlock-free mutual exclusion for $n$ threads.
> > [!info]- Proof (for $n = 2$)
> > Given two threads $A$, $B$ and one MRMW register $R$
> > Before $A$ or $B$ can enter its critical section, it must write to $R$.
> > Bring $A$ and $B$ in a position where they are both about to write to $R$, after which they perform reads, and may enter their critical sections.
> > Let $A$ write to $R$ first, perform reads, and enter its critical section.
> > The subsequent write by $B$ obliterates the value $A$ wrote to $R$, so $B$ can no longer tell that $A$ is in critical section.
> > $B$ also performs reads and enters its critical section.

> [!question] How does this proof idea carry over to general n?
> With only n − 1 registers, two threads must share a register to signal to other threads that they have entered their critical sections. Then the scenario from the proof applies.
## Deadlock-freeness refers to invoking `lock()`
The proof assumes that threads actively grab a lock, without being dependent on other threads.

Hence the next mutual exclusion algorithm for $n$ threads $A_0, . . . , A_{n−1}$ using one MRMW register is not a counterexample.

The MRMW register has a value in $\{0,...,n−1\}$.  
All threads spin on this register.  
If it has value $i$, thread $A_i$ can enter its critical section.

If $A_i$ does not want to enter or leaves its critical section, it writes the value $(i + 1) \mod n$ in the register.
# Fischer’s algorithm
- There are $n$ threads $A_{0},...,A_{n-1}$.
- *turn* is a MRMW register with range $\{-1,0,...,n-1\}$
- Initially it has the value -1.
- An $A_{i}$ wanting to enter its critical section, spins on turn until it is $-1$. **Within one time unit** of starting this read, $A_{i}$ completes writing the value $i$ to turn. 
- $A_{i}$ waits for **more than one time unit**, and then reads turn.
- If it still has the value $i$, then $A_{i}$ enters its critical section. Else $A_{i}$ returns to spinning on turn until it is $-1$.
- When a thread exits its critical section, it sets turn to $-1$.

> [!info] Correctness
> Fischer's algorithm guarantees **mutual exclusion**
> When $turn = -1$, no thread is in its critical section.
> If a thread sets turn, other threads can only concurrently set turn within one time unit of this first write.
> Since threads re-check turn one time unit after setting it, only the thread that set turn last will enter its critical section.
> 
> Fischer's algorithm is **deadlock-free**.
> When a thread exits its critical section, turn becomes $-1$.
> The last thread to set turn within one time unit of the first write enters its critical section.

> [!error] Drawbacks
> - Not starvation-free
> - Needless delay in case there is no contention
> - Requires a global clock
> - All threads spin on the same variable turn

---
References:
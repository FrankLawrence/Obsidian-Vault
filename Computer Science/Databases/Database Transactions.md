---
Tags: 
Created: 2024-05-31 02:50:02
---
(Links:: [[Databases]])
# Conflicts
Types of conflicts
There is a *conflict* between T1 and T2 if there is an item Y:
- **WR conflict**: T1 write Y and afterwards, T2 reads Y (If T1 has not committed this is a *dirty read*)
- **RW conflict**: T1 reads Y and afterwards, T2 writes Y (This read becomes _unrepeatable_.)
- **WW conflict**: T1 write Y and afterwards, T2 writes Y (This write becomes _overwritten_.)

Thus, actions _conflict_ if they:
- are from **different transactions**,
- involve the **same data item**, and
- one of the actions is a **write**.
# Serializability
> [!tip] Swapping non-conflicting actions
> We can **swap actions** (of different transactions) without changing the outcome, if the actions are non-conflicting.

> [!info] Conflict equivalence
> Two schedules are _conflict equivalent_ if they can be turned into each other by a sequence of non-conflicting swaps of adjacent actions.

> [!info] Conflict-serializable schedules
> A schedule is _conflict serializable_ if it is conflict equivalent to some serial schedule.
## Precedence Graphs
> [!info] Precedence graph
> Given a schedule we can create a _precedence graph_:
> - The graph has a node for each transaction.
> - There is an edge from transaction T1 to T2 if there is a conflicting action between T1 and T2 in which T1 occurs first.

## Conflict serializability via precedence graphs

> [!info] Conflict serializability via precedence graphs
> **If the precedence graph contains no cycles**, then the schedule is conflict serializable. Then an equivalent serial schedule is obtained by a _topological sort_ of the precedence graph.
# Two Phase Locking
### Lock-based Concurrency Control

One of the pessimistic strategies to ensure serializability is lock-based concurrency control: transactions must _lock_ objects before using them.

> [!info] Types of locks
> The types of locks are:
> - **Shared lock (S-lock)** is acquired on Y before reading Y. Many transactions can hold a shared lock on Y.
> - **Exclusive lock (X-lock)** is acquired on Y before writing Y. A transaction can hold an exclusive lock on Y only if no other transaction holds any lock on Y.

Note that:
- If a transaction has an X-lock on Y it can also read Y.
- Transactions unlocks objects when they are no longer needed.
## Deadlock Detection

## Cascading Rollbacks
### Strategies for dealing with cascading rollbacks

> [!info] Cascadeless
> **Delay reads:** only read values produced by already committed transactions.
> - If T2 reads a value written by T1, then the read is delayed until after the commit of T1.

With the cascadeless strategy there are no dirty reads, thus abort (rollback) does not cascade!

> [!info] Recoverable
> **Delay commits:**
> - If T2 reads a value written by T1, the commit of T2 is delayed until after the commit of T1.

**All transactions should be recoverable,** for otherwise we get inconsistent database states when a transaction is aborted. Note that cascadeless schedules are recoverable.

> All transactions must be recoverable.
## Strict Two Phase Locking

---
References:
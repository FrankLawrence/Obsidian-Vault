---
Tags: 
Created: 2024-05-14 17:02:13
---
(Links:: [[Databases]])
A transaction is a sequence of one or more actions performed on a database.

## ATM transaction

A client withdrawing 100 euro causes the ATM to perform a **transaction in the bank’s database**:

:: balance ← read(accountNr)  
balance ← balance - 100  
write(accountNr, balance)

The account is properly updated to reflect its new balance.

Formally, we define transactions as a list of read and write actions.

Definition

A **transaction** is a list of following actions

- reads R(O) and
    
- writes W(O)
    

performed on database objects O.

Transactions end with `Commit` or `Abort` (in MySQL, `ROLLBACK`).

|   |   |
|---|---|
|Note|In examples we sometimes omit `Commit` or `Abort` if they are not relevant for the example.|

#### Simple transaction example

|   |   |   |   |   |   |
|---|---|---|---|---|---|
     
|**T1:**|R(A)|R(B)|W(A)|W(C)|Commit|

### Interrupted transactions

What happens when transactions are interrupted (e.g., the plug is pulled)?

#### Interrupted transactions

Alice wants to transfer 500 Euros from her checking account to her saving account.

:: (Subtract money from source (checking) account)  
1: checking_balance ← read(checking_account_number)  
2: checking_balance ← checking_balance - 500  
3: write_balance(checking_account_number, checking_balance)  
  
(Add money to the target (saving) account)  
4: saving_balance ← read_balance(saving_account_number)  
5: saving_balance ← saving_balance + 500  
6: System crash!  
7: write_balance(saving_account_number, saving_balance)

Before the transaction gets to step (7), the execution is interrupted. For instance due to a power outage or a disk failure. Now **Alice lost 500 Euros!** The money has been deducted from the checking account, but not added to her saving account.

The example with the power outage shows that **unintended results can occur if transactions are partially, but not fully executed**.

A transaction should be an **atomic** event, that is, it should either be executed fully or not at all (but never partially).

The database management system takes care that transitions are atomic. If something goes wrong during the execution of a transaction (e.g. a power outage), the database management system will fully undo the transaction (the transaction is **aborted**).

### Concurrent access anomalies

When transactions concurrently operate on the same objects various **concurrency anomalies** arise. In the time between read and write operation on an object, the same objects can be read and/or modified by other transactions.

Important concurrency anomalies are:

- **Lost Update Anomaly:**  
    The effects of one transaction are lost due to an uncontrolled overwrite performed by a second transaction.
    
- **Dirty Read Anomaly:**  
    A transaction reads values change by another transaction which is aborted later (so the changes will be undone).
    
- **Non-repeatable Read Anomaly:**  
    A transaction reads a value which is concurrently changed by another transition, so a second read of the same object will result in a different value. The read is _not repeatable_.
    
- **Inconsistent Read Anomaly:**  
    A transaction reads the partial result of another transaction. So the former transition works data from an inconsistent database state.
    

We consider examples for each of these anomalies.

The following example illustrates that **concurrent transactions can influence each other by overwriting each others changes or reading results of partial (unfinished) concurrent transactions**. This leads to incorrect database updates and inconsistent database states.

#### Lost update anomaly (illustrated using ATM transactions)

Alice and Bob are married. They have a shared bank account and they each have a credit card for this account. What if they use the cards at the same time (concurrently)?

The account currently holds 1200 Euros, Bob withdraws 100 Euros, and Alice withdraws 200 Euros.

|   |   |   |
|---|---|---|
  
|Bob|Alice|DB state|
|balance ← read(accountNr)||1200|
||balance ← read(accountNr)|1200|
|balance ← balance - 100||1200|
||balance ← balance - 200|1200|
||write(accountNr,balance)|1000|
|write(accountNr,balance)||1100|

The update of Alice was lost during this execution. Lucky them!

Ideally, we want that the transactions do not influence each other. Although the transactions are executed in parallel, the result should be as if they had been executed in **isolation**, that is, sequentially (after one another).

|   |   |
|---|---|
|Caution|In case of multiple transactions (i.e. concurrent access), concurrent changes to objects should be coordinated with reads of the same objects in the database.|

This is the task of a **scheduler**. The scheduler decides how the transactions are executed and may decide to abort and undo (rollback) a transaction if there has been illegal interference between concurrent transactions.

A **dirty read** occurs if a transaction reads uncommitted changes made by another transaction which ends up being **aborted** later.

#### Dirty read anomaly

Recall that we write:

- R(O) for reading the database object O, and
    
- W(O) for writing the database object O.
    

     
|**T1:**|W(A)||||Abort|
|---|---|---|---|---|---|
|**T2:**||R(A)|…​|Commit||

Transaction 2 reads the value of V after this value has been change by transaction 1. However, transaction 1 ends up being aborted and thus all its effects will be undone (rolled back). As a consequence, transaction 2 worked with a value of V that is never committed to the database.

The next example uses ATM transactions to illustrate the dirty read anomaly.

#### Dirty read anomaly (illustrated using ATM transactions)

Again, Alice and Bob are doing concurrent ATM transaction.

|   |   |   |
|---|---|---|
  
|Bob|Alice|DB state|
|balance ← read(accountNr)||1200|
|balance ← balance - 100||1200|
|write(accountNr,balance)||1100|
||balance ← read(accountNr)|1100|
||balance ← balance - 200|1100|
|**abort**||1200|
||write(accountNr,balance)|900|

Bob’s transaction gets cancelled short before being finished! Alice’s transaction has already read the modified account balance before Bob’s transaction was rolled back (i.e., the effects are undone).

An **non-repeatable read** occurs if a transaction reads a data object which is concurrently modified by another transaction.

#### Non-repeatable read anomaly

Let’s consider simple example of the following two concurrent transactions.

      
|**T1:**|R(A)|||R(A)|…​|Commit|
|---|---|---|---|---|---|---|
|**T2:**||W(A)|Commit||||

After the first transaction reads the database object V, the second transaction modifies V and commits the change to the database. So first transaction computes with a stale value of V. Reading V again results in a different value: the read is _not repeatable_.

An **inconsistent read** occurs if a transaction reads the partial results of another transaction.

#### Inconsistent read anomaly

Let’s consider simple example of the following two concurrent transactions.

      
|**T1:**|W(A)|||W(B)|…​|Commit|
|---|---|---|---|---|---|---|
|**T2:**||R(A)|R(B)||…​|Commit|

The second transaction reads the partial result of changes of the first transaction. So the second transaction works with data from an inconsistent database state.

#### Inconsistent read anomaly (illustrated using ATM transactions)

Consider two concurrent transactions:

- Transaction 1 transfers 500 Euros from the checking to the saving account.
    
- Transaction 2 computes the total balance of checking together with saving.
    

The second transaction occurs right in the middle of the first transaction as follows:

|Transaction 1|Transaction 2|
|---|---|
|1. UPDATE Accounts<br>2. SET balance = balance-500<br>3. WHERE customer = 1904<br>4.   AND account_type = ’C’||
||1. SELECT SUM(balance)<br>2. FROM Accounts<br>3. WHERE customer = 1904|
|5. UPDATE Accounts<br>6. SET balance = balance+500<br>7. WHERE customer = 1904<br>8.   AND account_type = ’S’||

In this example:

1. Transaction 1 updates balance of the checking account.
    
2. Transaction 2 reads and computes the sum of checking and savings balance.
    
3. Transaction 1 updates the balance of the saving account and commits.
    

If Transaction 2 sees a temporary, inconsistent database state: the checking balance being decreased while the saving balance has not been increased yet. As a consequence, transaction 2 makes incorrect calculations.

# ACID properties

To protect from the problems and anomalies illustrated above, the database management system ensures four standard properties of transactions, the so-called **ACID properties**.

> [!info]- ACID properties
> - **Atomicity:** transaction executes fully (commit) or not at all (abort);
> - **Consistency:** transactions always leave the database in a consistent state where all defined integrity constraints hold;
> - **Isolation:** multiple users can modify the database at the same time but will not see each others partial actions;
> - **Durability:** once a transaction is committed successfully, the modified data is persistent, regardless of disk crashes.

Database management systems offer different levels of isolation. Serializability is a level of transaction isolation that prohibits dirty reads and other possible anomalies.

---
References:
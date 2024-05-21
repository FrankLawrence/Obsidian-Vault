---
Tags: 
Created: 2024-04-28 02:00:49
---
(Links:: [[Databases]])
# Basic Syntax of SQL
A basic SQL query follows like this:
$$\begin{align}
&\textrm{SELECT}\quad A_{1},...,A_{n} \\
&\textrm{FROM}\quad \;\;\;R_{1},...,R_{m} \\
&\textrm{WHERE}\quad \,C
\end{align}$$

- The `FROM` clause declares which **table(s)** are accessed.
- The `WHERE` clause specifies a **condition** for rows in these tables that are considered in this query. The absence of $C$ is equivalent to `true`.
- The `SELECT` clause specifies the attributes of the **result**. Here `*` means output all attributes occurring in $R_{1},\ldots,R_{m}$

The `FROM` clause can be understood as **declaring variables** that **range over tuples** of a relation. You can then query for tuples, where the elements within meet a certain criteria given be the `WHERE` clause (like an `if` statement).

For each table in the `FROM` clause there is a tuple variable. If the name of the tuple variable is not given explicitly, the variable will have the name of the relation. 

> [!example]-
> ```SQL
> SELECT E.number, E.topic
> FROM Exercises E     -- Leaving out E, will require changing all occurrences of the tuple variable with 'Exercises'
> WHERE E.category = 'homework'
> ```
> |number|topic|
> |---|---|
> |1|Logic|
> |2|SQL|

## Attribute References
Let $R$ be a tuple variable and $A$ an attribute of $R$. Attributes are referenced in the form $$R.A$$ If $R$ is the *only* tuple variable with attribute $A$, then it suffices to write $A$

> [!example]-
> Consider the two relations: 
> - `Students(sid,first,last,address)` and 
> - `Results(sid,category,number,points)`
> 
> A valid query for all students named "George Orwell", and their points would look as follows:
> ```SQL
> SELECT   category, number, points
> FROM     Students S, Results R
> WHERE    S.sid = R.sid
> 	AND first = 'George' AND last = 'Orwell'
> ```
> |category|number|points|
> |---|---|---|
> |exam|1|12|
> |homework|1|10|
> |homework|2|8|

## Query Formulation Traps

> [!tip] Typical mistakes
> - **Missing join conditions**
> - **Unnecessary joins** (may slow query down significantly)
> - **Self joins**: incorrect treatment of multiple variables ranging over the same relation (*missing (in)equality conditions*)
> - **Unexpected duplicates**, often an indicator for faulty queries (adding `distinct` is no cure here).
> - **Unnecessary `distinct`** (may slow query down)
# Joins
If we consider two tuple variables, with each 6 rows, then all $6\times 6=36$ row combinations will be considered when comparing with the `WHERE` clause. A good DBMS (Data Base Management System) will use a **better evaluation algorithm** (depending on the condition $C$). This is the task of the *query optimiser*. For understanding the [[semantics]] of a query, the simple nested *foreach algorithm* suffices.

A **join** needs to be explicitly specified in the `WHERE` clause: 
```SQL
SELECT    category, number, points
FROM      Students S, Results R
WHERE     S.sid = R.sid        -- Join Condition
	AND first = 'George' AND last = 'Orwell'
```
|category|number|points|
|---|---|---|
|exam|1|12|
|homework|1|10|
|homework|2|8|

> [!error]
> It is almost always an **error** if there are two tuple variables which are **not linked** (directly or indirectly) via join conditions.

Typically, the conditions correspond to the **foreign key relationships** between the tables.

> [!question]- Exercise
> Consider the following relations:
> - `Students(sid, first, last, address)`
> - `Exercises(category, number, topic, maxPoints)`
> - `Results(sid -> Students, (category, number) -> Exercises, points)`
> 
> Formulate the following query in SQL: The topics of all exercises solved by George Orwell
> 
> > [!tip]-
> > The **connection graph** with foreign key relations:
> > ```mermaid
> > flowchart LR
> > 	a(Students) --- b(Results) --- c(Exercises)
> > ```
> 
> > [!done]- Solution
> > To link the `Students` relation with `Exercises`, we must establish a link via tuple variable `R` over `Results`:
> > ```SQL
> > SELECT  E.topic
> > FROM    Students S, Exercises E, Results R
> > WHERE   S.first = 'George' AND S.last = 'Orwell'
> > 	AND S.sid = R.sid
> > 	AND R.category = E.category AND R.number = E.number
> > ```
> > |topic|
> > |---|
> > |SQL|
> > |Logic|
> > |SQL|
## Unnecessary Joins
> [!warning]
> Do not join **more** tables than needed.

> [!question]+ Exercise
> Consider the following relations and the given query:
> - `Students(sid, first, last, address)`
> - `Exercises(category, number, topic, maxPoints)`
> - `Results(sid -> Students, (category, number) -> Exercises, points)`
> 
> ```SQL
> SELECT   R.sid, R.points
> FROM     Results R, Exercises E
> WHERE    R.category = E.category AND R.number = E.number
> 	AND  E.category = 'homework' AND E.number = 1
> ```
> Will the following query produce the same Results?
> ```SQL
> SELECT   sid, points
> FROM     Results R
> WHERE    category = 'homework' and number = 1
> ```
> > [!done]- Solution
> > Yes!
> > 
> > |sid|points|
> > |---|---|
> > |101|10|
> > |102|9|
> > |103|5|

> [!question]+ Exercise
> Is there any difference between these two queries?
> ```SQL
> SELECT    S.first, S.last
> FROM      Students S
> ```
> ```SQL
> SELECT    distinct S.first, S.last
> FROM      Students S, Results R
> WHERE     S.sid = R.sid
> ```
> > [!done]- Solution
> > In the second query, people with the *same first name*, but not necessarily the same last name will be will be left out. Also, only those students who have results will be shown (which not all must have).
> > 
> > |first|last|
> > |---|---|
> > |George|Orwell|
> > |Elvis|Presley|
> > |Lisa|Simpson|
> > |Bart|Simpson|
> > |George|Washington|
> > 
> > |first|last|
> > |---|---|
> > |George|Orwell|
> > |Elvis|Presley|
> > |Lisa|Simpson|

# Self Joins
In some query scenarios, we might have to consider **more than one tuple of the same relation** to generate a result tuple.

> [!question]- Is there a student with 9 points for both, homework 1 & 2?
> ```SQL
> SELECT   S.first, S.last
> FROM     Students S, Results H1, Results H2
> WHERE    S.sid = H1.sid and S.sid = H2.sid
> 	AND  H1.category = 'homework' AND H1.number = 1
> 	AND  H2.category = 'homework' AND H2.number = 2
> 	AND  H1.points = 9 AND H2.points = 9
> ```
> |first|last|
> |---|---|
> |Elvis|Presley|

> [!question]- Students that solved at least two homework assignments
> ```SQL
> SELECT   S.first, S.last
> FROM     Students S, Results R1, Results R2
> WHERE    S.sid = R1.sid AND R1.category = 'homework'
> 	AND  S.sid = R2.sid AND R2.category = 'homework'
> ```
> |first|last|
> |---|---|
> |George|Orwell|
> |George|Orwell|
> |Elvis|Presley|
> |Elvis|Presley|
> |Lisa|Simpson|
> |George|Orwell|
> |George|Orwell|
> |Elvis|Presley|
> |Elvis|Presley|
> > [!warning] We need to ensure that `R1` and `R2` refer to distinct results:
> > ```SQL
> > ...
> > AND (R1.sid <> R2.sid OR
> > 	 R1.category <> R2.category OR
> > 	 R1.number <> R2.number)
> > ```
> > |first|last|
> > |---|---|
> > |George|Orwell|
> > |Elvis|Presley|
> > |George|Orwell|
> > |Elvis|Presley|

# Duplicate Elimination
A core difference between SQL and relational algebra is that **duplicates have to be explicitly eliminated** in SQL. The `distinct` modifier may be applied to the `SELECT` clause to request explicit duplicate row elimination. You should avoid unnecessary duplicate elimination.

> [!info] Sufficient condition for superfluous `distinct`
> Assumption: `WHERE` clause is a conjunction (`AND`)
> 1. Let $\mathcal{K}$ be the set of attributes in the `SELECT` clause.
> 2. If $A=c$ in the `WHERE` clause and $c$ a constant, add $A$ to $\mathcal{K}$.
> 3. If $A=B$ in the `WHERE` clause and $B\in \mathcal{K}$, add $A$ to $\mathcal{K}$
> 4. If $\mathcal{K}$ has a key of a variable $X$, add all attributes of $X$ to $\mathcal{K}$.
> 5. Repeat 2,3 and 4 until $\mathcal{K}$ is stable.
> 
> If $\mathcal{K}$ **contains a key of every tuple variable** listed under `FROM`, then `distinct` is superfluous.
> > [!example]-
> > ```SQL
> > SELECT   distinct S.first, S.last, R.number, R.points
> > FROM     Students S, Results R
> > WHERE    R.category = 'homework' AND R.sid = S.sid
> > ```
> > |first|last|number|points|
> > |---|---|---|---|
> > |George|Orwell|1|10|
> > |George|Orwell|2|8|
> > |Elvis|Presley|1|9|
> > |Elvis|Presley|2|9|
> > |Lisa|Simpson|1|5|
> > Let us assume that $\{\textsf{ first, last }\}$ is a key for $\textsf{Students}$.
> > 1. Initialiste $\mathcal{K}=\{\textsf{ S.first, S.last, R.number, R.points }\}$
> > 2. $\mathcal{K}+\{\textsf{ R.category }\}$ because of $\textsf{R.category}=\textsf{'homework'}$
> > 3. $\mathcal{K}+\{\textsf{ S.sid, S.address }\}$ since $\mathcal{K}$  contains a key of $\textsf{Students}$
> > 4. $\mathcal{K}+\{\textsf{ R.sid }\}$ because of the conjunct $\textsf{S.sid}=\textsf{R.sid}$
> > 
> > Finally $\mathcal{K}$ contains a key of
> > - $\textsf{Students S \{S.first, S.last\}}$ and
> > - $\textsf{Results R \{R.sid, R.cat, R.eno\}}$
> > 
> > Thus `distinct` is superluous
> > > [!warning] If $\mathsf{\{ first, last\}}$ is not key of $\mathsf{Students}$, the text would fail. Rightly so, since then the result could contain duplicates.
# Non-Monotonic Queries
SQL queries using only the constructs introduced so far compute **monotonic functions** on the database state:
- if further rows get **inserted**, these queries yield a **superset** of rows

However, not all queries behave monotonically in this way

> [!example]+ 
> Query: find students who have not submitted any homework.
> - Currently, `Bart Simpson` would be a correct answer.
> - This answer is no longer valid after:
> > ```SQL
> > insert into Results values (104, 'homework', 1, 8)
> > ```

In natural language, queries that contain formulations like 
- 'there is no' / 'does not exists' -> negated [[existential quantification]]
- 'for all' / 'the minimum/maximum' -> [[universal quantification]]

indicate non-monotonic behaviour.
> [!info] In an equivalent SQL formulation of such queries, this boils down to a **test whether a query yields a (non-) empty result**
# (Not) In
With `IN` and `NOT IN`, it is possible to check whether an attribute value appears in a set of values computed by another SQL **subquery**.

> [!example] Students without any homework result
> ```SQL
> SELECT   first, last
> FROM     Students
> WHERE    sid NOT IN (SELECT   sid
> 					FROM      Results
> 					WHERE     category = 'homework')
> ```
> |first|last|
> |---|---|
> |Bart|Simpson|
> |George|Washington|

The **subquery** is evaluated before the **main query**. This can be visualised as two seperate tables.

![[57862.png|500]]

![[56322.png|300]]

Then, for every tuple of `Students`, a matching `sid` is search in the subquery result. If there is none, the tuple is not part of the output.

> [!example] Topics of homework tasks solved by at least one student
> - `Exercises(category, number, topic, maxPoints)`
> - `Results(sid, category, number, points)`
> 
> ```SQL
> SELECT  topic
> FROM    Exercises
> WHERE   category = 'homework'
> 	AND number IN (SELECT  number
> 				   FROM    Results
> 				   WHERE   category = 'homework')
> ```
> |topic|
> |---|
> |Logic|
> |SQL|
> > [!question]- Is there a difference to this query?
> > ```SQL
> > SELECT  distinct topic
> > FROM    Exercises E, Results R
> > WHERE   E.category = 'homework'
> > 	AND R.category = 'homework'
> > 	AND E.number = R.number
> > ```
> > > [!tip]- Solution
> > > No!

# (Not) Exists
The construct `NOT EXISTS` enables the main (or outer) query to check whether the **subquery result is empty**.

> [!example] Students that have not submitted any homework
> ```SQL
> SELECT  first, last
> FROM    Students S
> WHERE   NOT EXISTS (SELECT  *
> 					FROM    Results R
> 					WHERE   R.category = 'homework'
> 					AND     R.sid = S.sid)
> ```
> |first|last|
> |---|---|
> |Bart|Simpson|
> |George|Washington|

In the subquery, tuple variables declared in the `FROM` clause of the outer query may be referenced (but the **converse is illegal**!). The subquery is said to be "*parameterized*"/"correlated". In the above example, conceptually the tuple variable `S` loops over the 5 rows in `Students`. As a result, the subquery is evaluated 5 times.
You can imagine the subquery as a stand alone query by replacing `S.sid` with a fixed value (such as 101).
When creating nested queries, it is always better to start by creating the subquery, and then forming the outer query.

> [!error] **Non-correlated subqueries** with `NOT EXISTS` are almost always an indication of an error!

Non-correlated subqueries evaluate to a set/relation **constant** and may make perfect sense (e.g. when used with `(NOT) IN`).

> [!question]+ Can we reformulate the below query without using `EXISTS`?
> ```SQL
> -- Who has submitted at least one homework?
> SELECT  sid, first, last
> FROM    Students S
> WHERE   EXISTS (SELECT  *
> 				FROM    Results R
> 				WHERE   R.category = 'homework'
> 				AND     R.sid = S.sid)
> ```
> |sid|first|last|
> |---|---|---|
> |101|George|Orwell|
> |102|Elvis|Presley|
> |103|Lisa|Simpson|
> > [!tip]- Solution
> > ```SQL
> > SELECT  sid, first, last
> > FROM    Students S
> > WHERE   sid IN (SELECT  sid
> > 				FROM    Results R
> > 				WHERE   R.category = 'homework')
> > ```
# For All & Implication
$\exists X(\varphi)$ ($\exists$: [[Existential Quantification]]) shows that there is an $X$ that satisfies formula $\varphi$. Similarly, $\forall X(\varphi)$ ($\forall$: [[Universal Quantification]]) shows that for all $X$, formula $\phi$ is satisfied. SQL does *not* offer a universal quantifier, but only the existential quantifier `EXISTS`. This proves no problem since we know that $$\forall X(\varphi)\quad\Longleftrightarrow\quad\neg\exists X(\neg\varphi)$$
SQL does also not have $\Rightarrow$. The commonly used pattern $$\forall X(\alpha\Rightarrow \beta)$$ becomes $$\neg\exists X(\alpha\land\neg\beta)$$

> [!question]- Who got the best result for homework 1?
> In natural language: the students $S$ that have a result $X$ for homework 1 such that for all result $Y$ for homework 1 it holds that $Y.points$ is less or equal to $X.points$.
> This can be formulated in predicate logic (tuple relational calculus): 
> $$\begin{align}
> \{S\mid & \;S\in Students \land X \in Results \land S.sid = X.sid \\
> & \land\; X.category = \text{'}homework\text{'} \land X.number = 1\\
> & \land\;\forall Y((Y\in Results \land Y.category = \text{'}homework\text{'}\land Y.number = 1)\\
> &\qquad\quad\Rightarrow Y.points \leq X.points )\}
> \end{align}$$
> Since $\forall X(\alpha\Rightarrow \beta)\equiv\neg\exists X(\alpha\land\neg\beta)$, we can also write:
> $$\begin{align}
> \{S\mid & \;S\in Students \land X \in Results \land S.sid = X.sid \\
> & \land\; X.category = \text{'}homework\text{'} \\
> & \land\;\neg\exists Y((Y\in Results \land Y.category = \text{'}homework\text{'}\land Y.number = 1)\\
> &\qquad\quad\;\;\land Y.points > X.points )\}
> \end{align}$$
> > [!tip]- Solution
> > ```SQL
> > SELECT  first, last, points  
> > FROM    Students S, Results X
> > WHERE   S.sid = X.sid
> > 	AND X.category = 'homework' AND X.number = 1
> > 	AND not exists
> >         (SELECT *
> > 	    FROM   Results Y
> >         WHERE  Y.category = 'homework' AND Y.number = 1
> > 	        AND Y.points > X.points)
> > ```
> > |first|last|points|
> > |---|---|---|
> > |George|Orwell|10|
> 
> > [!tip]- Alternative Solution
> > ```SQL
> > SELECT first, last, points
> > FROM Students S, Results X
> > WHERE S.sid = X.sid AND X.category = 'homework' AND X.number = 1
> >     AND points >= (SELECT MAX(points)
> > 	               FROM   Results Y
> > 		           WHERE  Y.category = 'homework' AND Y.number = 1)
> > ```

# Nested Subqueries
Subqueries may be nested.
> [!example]+ List the students who solved all homework assignments
> ```SQL
> SELECT   first, last
> FROM     Students S
> WHERE    NOT EXISTS (SELECT  *
> 					 FROM    Exercises E
> 					 WHERE   category = 'homework'
> 						 AND NOT EXISTS (SELECT  *
> 										 FROM    Results R
> 										 WHERE   R.sid = S.sid
> 											 AND R.number = E.number
> 											 AND R.category = E.category))
> ```
> |first|last|
> |---|---|
> |George|Orwell|
> |Elvis|Presley|
> Inner query: All results for student $S$ and homework $E$.
> Middle query: Homework of student $S$ for which no result exists.
> Outer query: Students that have no homework without results.
# All, Any, Some
SQL allows to compare *single value* with all values computed by a *single-column* subquery. Such comparisons may be universally or existentially quantified.

> [!question]- Who got the best result for homework 1?
> ```SQL
> SELECT   S.first, S.last, X.points
> FROM     Students S, Results X
> WHERE    S.sid = X.sid
> 	AND X.category = 'homework' AND X.number = 1
> 	AND X.points >= ALL (SELECT   Y.points
> 						 FROM     Results Y
> 						 WHERE    Y.category = 'homework'
> 							AND  Y.number = 1)
> ```
> |first|last|points|
> |---|---|---|
> |George|Orwell|10|
> > [!tip]- Alternative Solution from previous query (but with `ANY`)
> > SELECT   S.first, S.last, X.points
> > FROM     Students S, Results X
> > WHERE    S.sid = X.sid
> > 	AND X.category = 'homework' AND X.number = 1
> > 	AND NOT X.points < ANY (SELECT   Y.points
> > 						 FROM     Results Y
> > 						 WHERE    Y.category = 'homework'
> > 							AND  Y.number = 1)
> > ```

The subquery must yield a **single result column**!
> [!info] $\{\text{ALL, ANY, SOME}\}$ do **not** extend SQL's expressiveness
> The statement $$A < \text{ANY} \;(\text{SELECT \;B\; FROM\; $...$\; WHERE \;$...$\;})$$ is equivalent to $$\text{EXISTS}\; (\text{SELECT \;B\; FROM \;$...$\; WHERE \;$...$\; AND}\; A<B)$$
> ---
> The statement $$x \text{ in } S$$ is equivalent to $$x = \text{ any } S$$
# Single Value Subqueries
If **none of the keywords** `ALL, ANY, SOME` are present, i.e. $$...\;\text{WHERE } \;x = (\text{SELECT \;A \;FROM \;}...),$$ the subquery must yield *single column and* **at most one row**. So the comparison is between atomic values.
> [!question]- Who got full points for homework 1?
> ```SQL
> SELECT  S.first, S.last
> FROM    Students S, Results R
> WHERE   S.sid = R.sid AND category = 'homework' AND number = 1
> 	AND points = (SELECT  maxPoints
> 				  FROM    Exercises
> 				  WHERE   category = 'homework' AND number = 1)
> ```
> |first|last|
> |---|---|
> |George|Orwell|
> > [!question]- Why is this query guaranteed to return a single column & row?
> > Because *category* and *number* together form a primary key, which returns a tuple. maxPoints dictates which column of the tuple should be returned.

Use constraints to ensure that the query returns only one row! If the subquery has an empty result, the *null value* is returned.
# Views & Subqueries under From
Since an SQL query returns a table, it makes sense to use a subquery wherever a table might be specified. SQL allows subqueries *in the `FROM` clause*.

> [!question]- Points (in %) achieved in homework exercise 1
> ```SQL
> SELECT  X.sid, (X.points * 100 / X.maxPoints) as percent
> FROM    (SELECT  E.category, E.number, R.sid, R.points, E.maxPoints
> 		 FROM    Exercises E, Results R
> 		 WHERE   E.category = R.category AND E.number = R.number) X
> WHERE   X.category = 'homework' AND X.number = 1
> ```
> |sid|percent|
> |---|---|
> |101|100.0000|
> |102|90.0000|
> |103|50.0000|

One use of subqueries under `FROM` are **nested aggregations**. 

> [!warning] Inside the subquery, tuple variables introduced in the same `FROM` clause **may not be referenced**.
> ```SQL
> SELECT  S.first, S.last, X.number, X.points
> FROM    Students S, (SELECT  R.number, R.points
> 					 FROM    Results R
> 					 WHERE   R.category = 'homework'
> 						 AND R.sid = S.sid) X     -- Fails here!
> ```

If you do not want to create the table as a subquery, a **view declaration** registers a **query (not a query result)** under a given identifier in the database.
> [!example]-
> ```SQL
> CREATE  VIEW HomeworkPoints AS
> SELECT  S.first, S.last, R.number, R.points
> FROM    Students S, Results R
> WHERE   S.sid = R.sid AND R.category = 'homework'
> ```

# Aggregation Functions
Aggregation functions are functions from a set or multiset to a single value, e.g., $$min\{42,57,5,12,27\}=5$$ They take as input the values of an entire column. Aggregation functions are also known as *group functions*/*column functions*. SQL-92 defines 5 aggregation functions: `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`.
> [!question]- How many Students in the current database state?
> ```SQL
> SELECT  COUNT(*)
> FROM    Students
> ```
> |COUNT(*)|
> |--------|
> |5       |

Some aggregation functions are sensitive to **duplicates**: `SUM`, `COUNT`, `AVF`. Some are insensitive: `MIN`, `MAX`. You can request to ignore these: `COUNT(DISTINCT A)`. Simple aggregations feed the value set of an entire column into an aggregation function.

> [!question]- Best and average result for homework 1?
> ```SQL
> SELECT  MAX(points), AVG(points)
> FROM    Results
> WHERE   category = 'homework' AND number = 1
> ```
> 
> | MAX(points) | AVG(points) |
> |-------------|-------------|
> | 10          | 8           |

> [!question]- How many Students have submitted a homework?
> ```SQL
> SELECT  COUNT(DISTINCT sid)
> FROM    Results
> WHERE   category = 'homework'
> ```
> 
> | COUNT(DISTINCT sid) |
> |---------------------|
> | 3                   |

> [!question]- What is the total number of points student 101 got for her homework?
> ```SQL
> SELECT  SUM(points)
> FROM    Results R
> WHERE   sid = 101 AND R.category = 'homework'
>```
> | SUM(points) |
> |-------------|
> | 18          |

SQL also allows to write formulas.

> [!question]- What average percentage of the maximum points did the students reach for homework 1
> ```SQL
> SELECT  AVG(R.points / E.maxPoints) * 100
> FROM    Results R, Exercises E
> WHERE   R.category = 'homework' AND R.number  = 1
>     AND E.category = 'homework' AND E.number = 1
> ```
> |AVG(R.points / E.maxPoints) * 100|
> |---------------------------------|
> |80.00000000                      |

> [!question]- Homework points for student 101 plus 3 bonus points
> ```SQL
> SELECT  SUM(points) + 3 AS "total homework points"
> FROM    Results
> WHERE   sid = 101 AND category = 'homework'
> ```
> |total homework points|
> |---------------------|
> |21                   |

> [!error]
> Aggregations may not be used in the `WHERE` clause:
> ```SQL
> ... WHERE SUM(A) > 100 ...
> ```
> If an aggregation function is used without `GROUP BY`, **no attributes** may appear in the `SELECT` clause:
> ```SQL
> SELECT  category, number, AVG(points)
> FROM    Results
> ```

Usually, null values are ignored (filtered out) before the aggregation operator is applied. The exception is:
- `COUNT(*)` counts null values
- `COUNT(*)` counts rows, not attribute values

If the input set is empty, aggregation functions yield `NULL`. The exception is `COUNT` returns 0 (since it counts rows).
# Aggregations with Group By and Having
`GROUP BY` **partitions** the rows of a table into **disjoint groups**: based on **value equality for the `GROUP BY` attributes**. Aggregation functions applied for each group separately. 
> [!example] Average points for each homework
> ```SQL
> SELECT   number, AVG(points)
> FROM     Results
> WHERE    category = 'homework'
> GROUP BY number
> ```

All tuples agreeing in their `number` values for a group:

| sid | category | number | points |
| --- | -------- | ------ | ------ |
| 101 | homework | 1      | 10     |
| 102 | homework | 1      | 9      |
| 103 | homework | 1      | 5      |
| --- | -------- | ------ | ------ |
| 101 | homework | 2      | 8      |
| 101 | homework | 2      | 9      |

The groups are formed **after** the evaluation of the `FROM` and `WHERE` clauses. Aggregation is subsequently done for every group (yielding as many rows as groups). The `GROUP BY` **never** produces empty groups. The `GROUP BY` attributes may be used in the `SELECT` clause since they have a **unique value for every group**. A reference to any other attribute is illegal. 

> [!question]- Is there any semantical difference between these queries?
> ```SQL
> SELECT   topic, AVG(points / maxPoints)
> FROM     Exercises E, Results R
> WHERE    E.category = 'homework' AND R.category = 'homework'
>     AND  E.number = R.number
> GROUP BY topic
> ```
> ```SQL
> SELECT   topic, AVG(points / maxPoints)
> FROM     Exercises E, Results R
> WHERE    E.category = 'homework' AND R.category = 'homework'
> 	AND  E.number = R.number
> GROUP BY topic, E.number
> ```
> > [!tip]- Solution
> > Yes!

Aggregations may not be used in the `WHERE` clause. With `GROUP BY`, however, it makes sense to **filter out entire groups** based on some aggregated group property. This is possible with SQL's `HAVING` clause.

> [!example]
> ```SQL
> SELECT   ...           -- output columns
> FROM     ...           -- what tuples
> WHERE    ...           -- filter tuples
> GROUP BY ...           -- group tuples
> HAVING   COUNT(*) > n  -- filter groups
> ```

The condition in the `HAVING` clause may (only) involve aggregation functions.

> [!question] Which Students got at least 18 homework points?
> ```SQL
> SELECT   first, last
> FROM     Students S, Results R
> WHERE    S.sid = R.sid AND R.category = 'homework'
> GROUP BY S.sid, first, last
> HAVING   SUM(points) >= 18
> ```
# Aggregation Subqueries
# Union & Case & Coalesce
"Union" allows to combine the results of two queries. This is needed since there is no other method to construct one result column that draws from different tables/columns. "Union" is necessary, for example, if specialisations of a concept ("subclasses") are stored in separate tables.
> [!example]
> If we have tables
> - `graduate_courses` and
> - `undergraduate_courses`
> 
> both of which are specialisations of the concept `course`.

> [!question] Total number of homework points for *every* student
> 
# Order By
# Left & Right Outer and Inner Joins

---
References:
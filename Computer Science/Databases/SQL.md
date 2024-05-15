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

> [!question]+
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

# Self Joins
In some query scenarios, we might have to consider **more than one tuple of the same relation** to generate a result tuple.

> [!question]- Is there a student with 9 points for both, homework 1 & 2?
> ```SQL
> SELECT   S.first, S.last
> FROM     Students S, Results H1, Results H2
> WHERE    S.sid = H.sid and S.sid = H2.sid
> 	AND  H1.category = 'homework' AND H1.number = 1
> 	AND  H2.category = 'homework' AND H2.number = 2
> 	AND  H1.points = 9 AND H2.points = 9
> ```

> [!question]- Students that solved at least two homework assignments
> ```SQL
> SELECT   S.first, S.last
> FROM     Students S, Results R1, Results R2
> WHERE    S.sid = R1.sid AND R1.category = 'homework'
> 	AND  S.sid = R2.sid AND R2.category = 'homework'
> ```
> > [!warning] We need to ensure that `R1` and `R2` refer to distinct results:
> > ```SQL
> > ...
> > AND (R1.sid <> R2.sid OR
> > 	 R1.category <> R2.category OR
> > 	 R1.number <> R2.number)
> > ```

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
> 
> > [!tip]- Alternative Solution
> > ```SQL
> > SELECT first, last, points
> > FROM Students S, Results R1
> > WHERE S.sid = R1.sid AND R1.category = 'homework' AND R1.number = 1
> >     AND points >= (SELECT MAX(points)
> >                 FROM   Results R
> >                 WHERE  R.category = 'homework' AND R.number = 1)
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
> 										 FROM    Results
> 										 WHERE   R.sid = S.sid
> 											 AND R.number = E.number
> 											 AND R.category = E.category))
> ```
> Inner query: All results for student $S$ and homework $E$.
> Middle query: Homework of student $S$ for which no result exists.
> Outer query: Students that have no homework without results.
# All, Any, Some
# Single Value Subqueries
# Views & Subqueries under From
# Aggregation Functions
# Aggregations with Group By and Having
# Aggregation Subqueries
# Union & Case & Coalesce
# Order By
# Left & Right Outer and Inner Joins

---
References:
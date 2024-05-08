---
Tags: 
Created: 2024-04-28 02:00:49
---
(Links:: [[Databases]])

SQL queries using only the constructs introduced so far compute **monotonic functions** on the database state:
- if further rows get **inserted**, these queries yield a **superset** of rows

However, not all queries behave monotonically in this way

> [!example] 
> Query: find students who have not submitted any homework.
> - Currently, `Bart Simpson` would be a correct answer.
> - This answer is no longer valid after:
> > ```SQL
> > insert into Results values (104, 'homework', 1, 8)
> > ```

In natural language, queries that contain formulations like 
- 'there is no' / 'does not exists' -> [[negated existential quantification]]
- 'for all' / 'the minimum/maximum' -> [[universally quantification]]

indicate non-monotonic behaviour.
> [!info] In an equivalent SQL formulation of such queries, this boils down to a **test whether a query yields a (non-) empty result**

---
References:
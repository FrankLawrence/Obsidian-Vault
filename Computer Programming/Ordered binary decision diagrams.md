---
Tags: lecture
Created: 2022-11-28 17:32:08
---
(Links:: [[Logic and Sets]])
# Binary decision tree
- Propositional variables `p`,`q` are called Boolean varialbes x,y
- A boolea function can be represented as a *binary decision tree*
	- dashed line to the left represents *0*
	- solid line to the right represents *1*
	- Non-leaves are *circles*
	- Leaves are *boxes*
# Ordered binary decision diagram (OBDD)
An OBDD is obtained by *collapsing* and *eliminating* nodes in a binary decision tree
- **C1**: All leaves labeled `0` are *collapsed*, and also all leaves labeled `1`
- **C2**: If the `0`-and `1`-edge of non-leaf *n* point to the same node *n'*,
	- then *n* is *eliminated*, and
	- its incoming edges are *redirected* to `n'`
- **C3**: (Two or more) non-leaves are *collapsed* to one node if:
	- they are associated with the same variable, and
	- their `0`-edges point to the same node, and
	  their `1`-edges point to the same node
## Reduced OBDDs
- An OBDD is called *reduced* if none of the rules C1, C2 and C3 can be applied to it
- Each order in which the rules C2 and C3 are applied leads to the same reduced OBDD
- A binary decision tree of a formula corresponds to its truth table:
  Each path from the root to a leaf captures one line in the truth table
  -> A reduced OBDD is a compressed representation of a truth table
- Different *reduced* OBDDs are never semantically equivalent
  -> semantic equivalence can be checked by the reduced OBDDs
  OBDD representations are sensitive to order change
# logical operations on OBDDs
- **Negation**: Swap 0 and 1 in the two leaf nodes
- **Disjunction**: 
	- Let the 0-edge from the root of $O_1$ lead to the OBDD $O_1'$.
	- Let the 1-edge from the root of $O_1$ lead to the OBDD $O_1''$.
	- Let the 0-edge from the root of $O_2$ lead to the OBDD $O_2'$.
	- Let the 1-edge from the root of $O_2$ lead to the OBDD $O_2''$.
	- The roots of $O_1$ and $O_2$ are associated to the same variable $x$
		- The root of the OBDD for $O_1 \lor O_2$ is associated to $x$

# Universal and existential quantification
$\forall x \;\phi$ is 1 if:
- $\phi$ with 1 substituted for $x$ yields 1, *and*
- $\phi$ with 0 substituted for $x$ yields 1

In other words, $\phi$ is true for all values of $x$

$\exists x \;\phi$ is 1 if:
- $\phi$ with 1 substituted for $x$ yields 1, *or*
- $\phi$ with 0 substituted for $x$ yields 1

In other words, $\phi$ is true for some values of $x$

| $y$ | $\forall x(x\to y)$ |
| --- | ------------------- |
| 1   | 1                   |
| 0   | 0                   |

$\forall x(x\to y)\equiv y$
There is no column for $x$ because its occurrence in $\forall x(x\to y)$ is *bound* by the universal quantification.
## Expressing quantification in propositional logic
Let $\phi_1$ and $\phi_o$ denote $\phi$ with 1 and 0 substituted for x, respectively
$\forall x \;\phi \equiv \phi_1 \land \phi_0$
## Universal quantification on OBDDs
We construct an OBDD $O(1)$ representing $O$ with 1 substituted for x:
- Eliminate all nodes from $O$ that are associated to x
- Redirect incoming edges of an eliminated node to the 1-child of this node. Or, if x is at the root, make its 1-child the root

We construct an OBDD $O(0)$ representing $O$ with 0 substituted for x:
- Eliminate all nodes from $O$ that are associated to x
- Redirect incoming edges of an eliminated node to the 0-child of this node. Or, if x is at the root, make its 0-child the root

$\forall x O$ is captured by the OBDD for $O(1)\land O(0)$
$\exists x O$ is captured by the OBDD for $O(1)\lor O(0)$

---
References:
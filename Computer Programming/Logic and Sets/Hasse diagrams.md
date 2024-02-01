---
Tags: 
Created: 2022-11-17 09:02:43
---
(Links:: [[Logic and Sets]])
- **Hasse diagram**: omit arrows implied by reflexivity and transitivity
- **Algorithm** (HAsse diagram for a given partial order $R$ on $V$)
	- for all $x\in V \qquad G_x:=\{y:y\neq x \text{ and } x\,R\,y\}$
	- for all $x\in V \qquad H_x:=G_x\setminus \bigcup_{y\in G_x}\,G_y$
	- for all $x\in V \qquad$ draw an arrow from $x$ to every $y\in H_x$
	  $\bigcup_{y\in G_x}\,G_y \quad$ is the **union** of all sets $G_y$ with $y\in G_x$

---
References:
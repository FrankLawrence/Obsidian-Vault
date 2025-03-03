---
tags: Math
---

| Nul $A$                                                                                                            | Col $A$                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| Nul $A$ is a subspace of $\Bbb R^{n}$                                                                              | Col $A$ is a subspace of $\Bbb R^{m}$                                                                                    |
| Nul $A$ is implicitly defined; that is, you are given only a condition (Ax=0) that vectors in Nul $A$ must satisfy | Col $A$ is explicitly defined; that is, you are told how to build vectors in Col $A$                                     |
| It takes time to find vectors in Nul $A$. Row operations on $[A\quad 0]$ are required.                             | It is easy to find vectors in Col $A$. The columns of $A$ are displayed; others are formed from them.                    |
| There is no obvious relation between Nul $A$ and the entries in $A$.                                               | There is an obvious relation between Col $A$ and the entries in $A$, since each column of $A$ is in Col $A$.             |
| A typical vector $v$ in Nul $A$ has the property that $Av=0$.                                                      | A typical vector $v$ in Col $A$ has the property that the equation $Ax=v$ is consistent.                                 |
| Given a specific vector $v$, it is easy to tell if $v$ is in Nul $A$. Just compute $Av$.                           | Given a specific vector $v$, it may take time to tell if $v$ is in Col $A$. Row operations on $[A\quad v]$ are required. |
| Nul $A=\{0\}$ iff the equation $Ax=0$ has only the trivial solution.                                               | Col $A=\Bbb R^{m}$ iff the equation $Ax=b$ has a solution for every $b$ in $\Bbb R^{m}$.                                 |
| Nul $A=\{0\}$ iff the linear transformation $x\mapsto Ax$ is one-to-one.                                           | Col $A=\Bbb R^{m}$ iff the linear transformation $x\mapsto Ax$ maps $\Bbb R^{n}$ *onto* $\Bbb R^{m}$                     |

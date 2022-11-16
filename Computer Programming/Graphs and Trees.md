---
Tags: 
Created: 2022-10-28 13:33:02
---
(Links:: [[Computational Thinking for Coders]])
# Connected and disconnected graphs

A graph is called a connected graph if, for any pair of its nodes $u$ and $v$, there is a path from $u$ to $v$. The example graph above is not connected, called disconnected, because there is no path by which node E can be reached.

# Directed and undirected graphs

Edges in a graph can be either directed or undirected. In the above example graph, all edges are undirected, connecting both their node $u$ with $v$ and $v$ with $u$. We speak of an undirected graph. The following example graph is directed: all its edges only connect their source $u$ to their respective destination $v$, but not the other way round. 

There are also mixed directed/undirected graphs. For simplicity we can think of such a graph as a directed one where each undirected edge is a shorthand for two directed edges, connecting the same pair of nodes, but in opposite directions.

# Directed graph connectivity

A directed graph is called weakly connected (or just "connected") if the underlying undirected graph (by replacing all directed edges by undirected ones connecting the same nodes) is connected. A directed graph is called strongly connected if, for each pair of nodes ($u$,$v$), there is both a directed path from $u$ to $v$ and from $v$ to $u$.

# Cyclic and acyclic graphs

A cycle in a graph is a non-empty path in which the only repeated nodes are the first and last nodes, and in which edges must not be repeated. A directed cycle in a directed graph is a non-empty directed path in which the only repeated nodes are the first and last nodes (and no edges are repeated). In the example of a directed graph above, there is a cycle A-C-B-A (or B-A-C-B, or C-B-A-C). A graph that contains one or more cycles is called a cyclic graph. 

A graph that does not contain cycles is called acyclic. Such acyclic graphs, esp. **directed acyclic graphs** (DAGs) are an important category as they allow to model hierarchical relationships amongst their nodes.

# Graph Representations
## Adjacency matrix for unweighted graphs

For unweighted graphs, the adjacency matrix is binary, with the value 1 indicating the presence of an edge, and the value 0 its absence. Undirected graphs have symmetrical matrices where $A[i,j]=A[j,i]$. The value $A[i,i]$ denotes whether or not there is an edge that leads from a node to itself.

# Tree
## Traversing the tree
Traversing a tree means visiting all its nodes in a particular order. Printing all keys in the tree in sorted order can be done by a so-called
- in-order traversal: For a tree, first traverse its left subtree, then print the key of the root node, and then traverse the right subtree.

Besides in-order traversal, also two other forms of traversal are possible for binary trees:
- pre-order traversal: Here, the root node is printed (or, in general, dealt with) first. Then, the two subtrees are traversed.
- post-order traversal: Here, the two subtrees are traversed first. Then, the root node is printed (or, in general, dealt with).
___
References:
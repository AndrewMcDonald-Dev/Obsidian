![[symbols.pdf]]

$\mathcal{u}$ = each connected node to the current node
$\pi$ = predecessor
$\mathcal{d}$ = discoveries?

G.V = All nodes
G.V - {s} = All nodes except the starting node

?.v = all neighbor nodes of ?

Initial State

|     | c   | d        | $\pi$       |
| --- | --- | -------- | ----------- |
| 0   | G   | 0        | $\emptyset$ |
| 1   | W   | $\infty$ | $\emptyset$ |
| 2   | W   | $\infty$ | $\emptyset$ |
| 3   | W   | $\infty$ | $\emptyset$ |
| 4   | W   | $\infty$ | $\emptyset$ |
| 5   | W   | $\infty$ | $\emptyset$ |
| 6   | W   | $\infty$ | $\emptyset$ |           |

Final State

|     | c   | d   | $\pi$       |
| --- | --- | --- | ----------- |
| 0   | B   | 0   | $\emptyset$ |
| 1   | B   | 1   | 0           |
| 2   | B   | 2   | 1           |
| 3   | B   | 3   | 2           |
| 4   | B   | 2   | 1           |
| 5   | B   | 1   | 0           |
| 6   | B   | 2   | 5            |

This result produces a tree

- No Loops
- Connected
- Number of Edges = Number of Vertices - 1 

Time Complexity?

Given: n = # of vertices
Initialization: $O(n)$
Everything else: $O(n^2)$ (worst case) 

We should actually use # of edges instead of vertices
For undirected graph each edge is counted twice and for directed each is counted once.

$O(E)$ this is for adjacency list. For matrix it is always $O(n^2)$. N is the number of nodes and E is the number of edges.


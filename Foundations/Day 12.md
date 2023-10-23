Have a take home exam which is a programming assignment.

Floyd-Warshall 

All Pairs Shortest Path

Input - directed, weighted graph
Output - Shortest Path from $V_i \rightarrow V_j$ 

We iterate over all nodes and check if these nodes will shorten the path of any other node to another.

$D^0$ 

|     | 0        | 1        | 2        | 3        | 4        | 5        |
| --- | -------- | -------- | -------- | -------- | -------- | -------- |
| 0   | 0        | 4        | $\infty$ | $\infty$ | $\infty$ | 2        |
| 1   | 1        | 0        | 3        | 4        | $\infty$ | $\infty$ |
| 2   | 6        | 3        | 0        | 7        | $\infty$ | $\infty$ |
| 3   | 6        | $\infty$ | $\infty$ | 0        | 2        | $\infty$ |
| 4   | $\infty$ | $\infty$ | $\infty$ | 5        | 0        | $\infty$ |
| 5   | $\infty$ | $\infty$ | $\infty$ | 2        | 3        | 0        | 

|     | 0   | 1   | 2   | 3   | 4   | 5   |
| --- | --- | --- | --- | --- | --- | --- |
| 0   |     |     |     |     |     |     |
| 1   |     |     |     |     |     |     |
| 2   |     |     |     |     |     |     |
| 3   |     |     |     |     |     |     |
| 4   |     |     |     |     |     |     |
| 5    |     |     |     |     |     |     |

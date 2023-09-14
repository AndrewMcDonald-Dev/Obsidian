
## Knapsack


|     | 0   | 1   | 2   | 3   | 4   | 5   | 6   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| 4   | 0   | 0   | 0   | 0   | 100 | 100 | 100 |
| 3   | 0   | 0   | 0   | 150 | 150 | 150 | 150 |
| 2   | 0   | 0   | 200 | 200 | 200 | 350 | 350 |
| 1   | 0   | 50  | 200 | 250 | 250 | 250 | 400 | 

|     | 0   | 1   | 2   | 3   | 4   | 5   | 6   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| 4   | 0   | 0   | 0   | 0   | 400 | 400 | 400 |
| 3   | 0   | 0   | 0   | 150 | 400 | 400 | 400 |
| 1   | 0   | 50  | 50  | 150 | 400 | 450 | 450 |
| 2   | 0   | 50   | 200 | 250 | 400 | 450 | 600 |

#### Solution 1: My attempt

1. Construct matrix of 0-capacity for x and list of weights for y.
2. Check weight with capacity if weight is higher than pull the value from above.
3. If it is lower take the value of the item and go `m[i-1][j-weight]`. Add the two values and then grab the max of that new value and the value above to get the answer.

It seems like this approach works. I can not think of a negative example. I think this is correct. Yup I am a god.


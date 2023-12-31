
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


|     | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  | 12  | 13  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| 5   | 0   | 0   | 0   | 0   | 0   | 10  | 10  | 10  | 10  | 10  | 10  | 10  | 10  | 10  |
| 3   | 0   | 0   | 0   | 20  | 20  | 20  | 20  | 20  | 30  | 30  | 30  | 30  | 30  | 30  |
| 8   | 0   | 0   | 0   | 20  | 20  | 20  | 20  | 20  | 30  | 30  | 30  | 45  | 45  | 45  |
| 4   | 0   | 0   | 0   | 20  | 20  | 20  | 20  | 28  | 30  | 30  | 30  | 45  | 45  | 45  |

5 = 10  3 = 20 8 = 25 4 = 8     8,3


## Subset Sum

|     | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| 3   | 0   | 0   | 0   | 1   |     |     |     |     |     |     |
| 34  | 0   |     |     |     |     |     |     |     |     |     |
| 4   | 0   |     |     |     |     |     |     |     |     |     |
| 12  | 0   |     |     |     |     |     |     |     |     |     |
| 5   | 0   |     |     |     |     |     |     |     |     |     |
| 2   | 0   |     |     |     |     |     |     |     |     |     |



```rust

fn main() {
	let set = [3, 34, 4, 12, 5, 2];
	let sum = 9;
	println!("{:?}", subset_sum(&set, sum, set.len()));

	let sum = 30;
	println!("{:?}", subset_sum(&set, sum, set.len()));
}

fn subset_sum(set: &[usize], sum: usize, n: usize) -> Vec<usize> {
	if sum == 0 {
			return vec![set[n]];
	}
	if n == 0 {
		return vec![];
	}
	if set[n - 1] > sum {
		subset_sum(set, sum, n - 1)
	} else {
	let mut positive_case = subset_sum(set, sum - set[n - 1], n - 1);

	// if positive case is good return that
	// if not return whatever negative case is
	let pos_len = positive_case.len();
	if pos_len != 0 {
		if !(pos_len == 1 && positive_case[0] == set[n - 1]){
			positive_case.push(set[n - 1]);
		}
		positive_case
	} else {
		subset_sum(set, sum, n - 1)
		}
	}
}
```
 
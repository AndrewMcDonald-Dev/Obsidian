More DP!

![[symbols.pdf]]

![[Latex Cheat Sheet]]

|     | 0        | 1        | 2        | 3        | 4        | 5        | 6        | 7        |
| --- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| 0   | $\infty$ | $\infty$ | $\infty$ | $\infty$ | $\infty$ | $\infty$ | $\infty$ | $\infty$ |
| 1   | $\infty$ | 1        | 2        | 3        | 4        | 5        | 6        | 7        |
| 3   | $\infty$ | 1        | 2        | 1        | 2        | 3        | 2        | 3        |
| 4   | $\infty$ |          |          |          |          |          |          |          |
| 5   | $\infty$ |          |          |          |          |          |          |          |
|     |          |          |          |          |          |          |          |          |

Coins Idea #1: In this case base case is 0.

1. Do $\frac{coin}{desired}$ 
2. If value is non-zero add that value to the total coins. 
	1. Then do $coin\mod desired$ and find that value for the previous coin `values[i-1][value]`.  
	2. Add the amount of coins together and get your result. 
	3. If this result is bigger than the answer above use that answer instead. Otherwise use our result.
3. If value is zero pull the value from above.

Coins Idea #2: `c = coin array`, i = index of c and j = desired value 

1. If $i | j =0$ then  $m[i][j] = \infty$.
2. If $c[i]>j$ then `m[i][j] = m[i-1][j]`.
3. If $c[i]=j$ then `m[i][j] = 1`.
4. If $c[i]<j$ then `m[i][j] = min(m[i - 1][j], m[i][j - c[i]] + 1)`.

The second one is simpler but I am unsure of what is more efficient. Both run times are $\mathcal{O} (n^2)$. I believe the 2nd answer is better because the $i>j$ and $i=j$ cases can and would benefit the first idea but aren't present. With these two extra cases the last case follows naturally and doesn't take any division or modulus operations only simple comparison. 
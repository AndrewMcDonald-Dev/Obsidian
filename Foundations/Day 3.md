
Dynamic Programming again

![[symbols.pdf]]

![[Latex Cheat Sheet]]


### Backtrack 

|     | 0   | c   | b   | a   | b   | a   | c   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| a   | 0   | 0   | 0   | 1   | 1   | 1   | 1   |
| b   | 0   | 0   | 1   | 1   | 2   | 2   | 2   |
| c   | 0   | 1   | 1   | 1   | 2   | 2   | 3   |
| a   | 0   | 1   | 1   | 2   | 2   | 3   | 3   | 
| b   | 0   | 1   | 2   | 2   | 2   | 3   | 3   |
| b   | 0   | 1   | 2   | 2   | 3   | 3   | 3   |
| a   | 0   | 1   | 2   | 3   | 3   | 4   | 4   |

Backtrack Idea #1: Follow the highest number.

1. Start at bottom right.
2. Move left or right depending on which is higher as long as it is the same as the last number.
3. Keep going till number changes in both directions and right down the letter match.
4. Repeat until reaching a 0.
5. Reverse String and you have your answer.

*Does not work*

Backtrack idea #2: Diagonal

1. Start at bottom right.
2. Move left or right depending on which is higher as long as it is the same as the last number.
3. When you hit a lone number mark it down and go to the top left diagonal and go again.
4. Repeat until reaching a 0.
5. Reverse String and you have your answer.

*Works*

Alternative Backtrack idea: Playback steps

1. Start at bottoms right.
2. Check if letters are a match.
3. If they are a match go to the top left, if not move left or right depending on which is higher as long as it is the same as the last number.
4. Repeat until reaching a 0.
5. Reverse string and you have your answer.

I believe this answer is *very* slightly better than mine on the average. There are scenarios where that is not the case.

Note: Both valid approaches can find all sub-sequences.

All max sub-sequences: `["caba", "cbba", "baba"]`

1. How long is the runtime for filling the graph?
	1. $O(n*m)$
2. How long is the runtime of a non-DP solution?
	1. $O(2^{n})$
3. How long does our backtracking take.
	1. For 1 answer?
		1. $O(max(n, m))$
	2. For all answers:
		1. $O(n*m)$



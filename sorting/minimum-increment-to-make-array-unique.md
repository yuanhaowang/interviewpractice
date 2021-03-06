#### Minimum Increment to Make Array Unique

> Given an array of integers A, a _move_ consists of choosing any `A[i]`, and incrementing it by `1`.
>
> Return the least number of moves to make every value in `A` unique.
>
> **Example 1:**
>
> ```
> Input: [1,2,2]
> Output: 1
> Explanation:  After 1 move, the array could be [1, 2, 3].
> ```
>
> **Example 2:**
>
> ```
> Input: [3,2,1,2,1,7]
> Output: 6
> Explanation:  After 6 moves, the array could be [3, 4, 1, 2, 5, 7].
> It can be shown with 5 or less moves that it is impossible for the array to have all unique values.
> ```
>
> **Note:**
>
> 1. `0 <= A.length <= 40000`
> 2. `0 <= A[i] < 40000`

##### Code \(Sorting\):

```py
def minIncrementForUnique(A):

    moves = need = 0
    for i in sorted(A):
        moves += max(need - i, 0)
        need = max(need + 1, i + 1)
    return res

```

The key constraints in this problem is that all numbers are nonnegative and we are only allowed to increment numbers. If the current number is the same as the previous one, then it needs to be incremented to prev + 1 in order for it to be unique. Running time is dominated by the sort, making it $\small \mathcal O(n \log{n})$.


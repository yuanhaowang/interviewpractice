#### Delete Columns to Make Sorted III

> We are given an array `A` of `N` lowercase letter strings, all of the same length.
>
> Now, we may choose any set of deletion indices, and for each string, we delete all the characters in those indices.
>
> For example, if we have an array `A = ["babca","bbazb"]` and deletion indices `{0, 1, 4}`, then the final array after deletions is `["bc","az"]`.
>
> Suppose we chose a set of deletion indices `D` such that after deletions, the final array has **every element \(row\) in lexicographic** order.
>
> For clarity, `A[0]` is in lexicographic order \(ie. `A[0][0] <= A[0][1] <= ... <= A[0][A[0].length - 1]`\), `A[1]` is in lexicographic order \(ie. `A[1][0] <= A[1][1] <= ... <= A[1][A[1].length - 1]`\), and so on.
>
> Return the minimum possible value of `D.length`.
>
> **Example 1:**
>
> ```
> Input: ["babca","bbazb"]
> Output: 3
> Explanation: After deleting columns 0, 1, and 4, the final array is A = ["bc", "az"].
> Both these rows are individually in lexicographic order (ie. A[0][0] <= A[0][1] and A[1][0] <= A[1][1]).
> Note that A[0] > A[1] - the array A isn't necessarily in lexicographic order.
> ```
>
> **Example 2:**
>
> ```
> Input: ["edcba"]
> Output: 4
> Explanation: If we delete less than 4 columns, the only row won't be lexicographically sorted.
> ```
>
> **Example 3:**
>
> ```
> Input: ["ghi","def","abc"]
> Output: 0
> Explanation: All rows are already lexicographically sorted.
> ```

##### Longest Increasing Subsequence:

```py
def minDeletionSize(A):

    n, m = len(A), len(A[0])

    dp = [1] * m

    for i in range(m):
        for j in range(i):
            if all(A[k][j] <= A[k][i] for k in range(n)):
                dp[i] = max(dp[j]+1, dp[i])

    return m - max(dp)
```

Refer to the Longest Increasing Subsequence for the idea behind the algorithm. 

The tricky part here was to realize that this problem could actually be modeled as an LIS problem. Since we have to delete entire columns, we essentially operate on all words at the same time. Which means every column kept must satisfy the order for all words, making it no different than if we were simply given one word. 

Running time is $$\small \mathcal O(m^{2}*n)$$. Space is $$\small \mathcal O(m)$$.


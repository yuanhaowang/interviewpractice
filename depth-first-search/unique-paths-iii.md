#### Unique Paths III

> On a 2-dimensional `grid`, there are 4 types of squares:
>
> * `1` represents the starting square.  There is exactly one starting square.
> * `2` represents the ending square.  There is exactly one ending square.
> * `0` represents empty squares we can walk over.
> * `-1` represents obstacles that we cannot walk over.
>
> Return the number of 4-directional walks from the starting square to the ending square, that **walk over every non-obstacle square exactly once**.
>
> **Example 1:**
>
> ```
> Input: [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
> Output: 2
> Explanation: 
> We have the following two paths: 
> 1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
> 2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
> ```
>
> **Example 2:**
>
> ```
> Input: [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
> Output: 4
> Explanation: 
> We have the following four paths: 
> 1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
> 2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
> 3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
> 4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
> ```
>
> **Example 3:**
>
> ```
> Input: [[0,1],[2,0]]
> Output: 0
> Explanation: 
>
> There is no path that walks over every empty square exactly once.
> Note that the starting and ending square can be anywhere in the grid.
> ```

##### DFS:

```py
def uniquePathsIII(grid):
    """
    :type grid: List[List[int]]
    :rtype: int
    """
    start, end, todo = None, None, 0
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == 1:
                start = (i, j)
            elif grid[i][j] == 2:
                end = (i, j)
            elif grid[i][j] == 0:
                todo += 1

    ans = 0
    def dfs(x, y, todo):
        nonlocal ans
        if x < 0 or y < 0 or x >= len(grid) or y >= len(grid[0]) or todo < 0:
            return 
        if (x,y) == end:
            if todo == 0:
                ans += 1
            return
        if grid[x][y] == -1:
            return        

        val = grid[x][y]
        grid[x][y] = -1

        dfs(x+1, y, todo-(val == 0))
        dfs(x, y+1, todo-(val == 0))
        dfs(x-1, y, todo-(val == 0))
        dfs(x, y-1, todo-(val == 0))   

        grid[x][y] = val

    dfs(start[0], start[1], todo)
    return ans
```

There is actually a DP solution, but it doesn't actually perform much better than the regular backtracking solution. A loose upper bound on runtime is $$\small O(4^{m*n})$$, where $$\small m,n$$ are the dimensions of the board. A tighter bound is $$\small O(4*3^{m*n}) = O(3^{m*n})$$ since only the 1st square actually has all 4 options open; all other squares only have 3 options, due to one of them being the previous cell.


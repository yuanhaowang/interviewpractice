#### Bag of Tokens

> You have an initial power `P`, an initial score of `0` points, and a bag of tokens.
>
> Each token can be used at most once, has a value `token[i]`, and has potentially two ways to use it.
>
> * If we have at least `token[i]` power, we may play the token face up, losing `token[i]` power, and gaining `1` point.
> * If we have at least `1` point, we may play the token face down, gaining `token[i]` power, and losing `1` point.
>
> Return the largest number of points we can have after playing any number of tokens.
>
> **Example 1:**
>
> ```
> Input: tokens = [100], P = 50
> Output: 0
> ```
>
> **Example 2:**
>
> ```
> Input: tokens = [100,200], P = 150
> Output: 1
> ```
>
> **Example 3:**
>
> ```
> Input: tokens = [100,200,300,400], P = 200
> Output: 2
> ```

##### Brute Force - Timed Out:

```py
def bagOfTokensScore(tokens, P):

    max_points = 0

    def helper(tokens, P, points):
        nonlocal max_points
        max_points = max(max_points, points)
        for i,t in enumerate(tokens):
            if t != 'U' and t != 'D' and P >= t:    # try playing up to gain a point
                tokens[i] = 'U'
                helper(tokens, P - t, points + 1)
                tokens[i] = t
            if points > 0 and t != 'D' and t != 'U':  # try playing down to gain P
                tokens[i] = 'D'
                helper(tokens, P + t, points - 1)
                tokens[i] = t

    helper(tokens, P, 0)
    return max_points
```

We can simply try to brute force all possible moves: 1\) if we have enough points, we can try to get more power, 2\) if we have enough power, we can try to get more points. Since there are 2 moves possible for each token, runtime blows to up exponential time, and we time out.

##### Greedy:

```py
def bagOfTokensScore(tokens, P):

    max_points = points = 0
    tokens.sort()

    start, end = 0, len(tokens) - 1
    while start <= end:
        if P >= tokens[start]:
            P -= tokens[start]
            points += 1
            start += 1
        elif points > 0:
            points -= 1
            P += tokens[end]
            end -= 1
        else:
            return max_points
        max_points = max(max_points, points)

    return max_points
```

The intuition is as follows: If we play a token face up, we might as well play the one with the smallest value. Similarly, if we play a token face down, we might as well play the one with the largest value.

We don't need to play anything until absolutely necessary.  We should always play tokens face up until exhaustion, then play one token face down and continue.

The final answer could be any of the intermediate answers we got after playing tokens face up \(but before playing them face down.\)


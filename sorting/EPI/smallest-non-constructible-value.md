#### Smallest Non-constructible Value

> Given a set of coins, there are some amounts of change that you may not be able to make with them, e.g., you cannot create a change amount greater than the sum of your coins. For example, if your coins are `1, 1, 1, 1, 1, 5, 10, 25` then the smallest value of change which cannot be made is 21.
>
> Write a program which takes an array of positive integers and returns the smallest number which is not equal to the sum of a subset of elements of the array.

##### Code \(Brute force\):

```py
def smallest_nonconstructible_value(A):
    possible_vals = set()
    for coin in A:
        new_set = {coin}
        for val in possible_vals:
            new_set.add(val + coin)
        possible_vals = possible_vals.union(new_set)

    i = 1
    while i in possible_vals:
        i += 1
    return i
```

##### Explanation:

We generate all possible constructible values, then simply iterate from `1` up until we find a value not in the set. Generating all should take factorial time, so obviously this not an ideal approach.

##### Code \(Optimized\):

```py
def smallest_nonconstructible_value(A):
    max_constructible_value = 0
    for a in sorted(A):
        if a > max_constructible_value + 1:
            break
        max_constructible_value += a
    return max_constructible_value + 1
```

##### Explanation:

The idea behind this algorithm is a behind dynamic programming-esque. Suppose we have a set of numbers that can produce every value up to and including $\small V$, but not $\small V + 1$. Let us then introduce a new element $\small u$ to the set. If $\small u \leq V + 1$, then we are able to produce every value up to and including $\small V + u$ and we cannot produce $\small V + u + 1$. However, suppose that $\small u \geq V + 1$. In this situation, we are still unable to produce $\small V + 1$, which is also the smallest nonconstructible value. 

While sorting the array does not change the set of constructible values, it does help us by alerting us as soon as we encounter a value that is too large to help.

Running time is $\small \mathcal O(n \log{n})$ for sorting. Iteration is masked by big-O notation. 


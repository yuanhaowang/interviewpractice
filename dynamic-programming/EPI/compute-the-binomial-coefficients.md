#### Compute the Binomial Coefficients

> The symbol$\binom{n}{k}$ is the short form for the expressions $\frac{n(n-1)...(n-k+1)}{k(k-1)...(3)(2)(1)}$. It is the number of ways to choose a $\small k-$element subset from an $\small n-$element set. It is not obvious that the expression defining $\small \binom{n}{k}$ always yields an integer. Furthermore, direct computation of $\small \binom{n}{k}$ from this expression quickly results in the numerator or denominator overflowing if integer types are used, even if the final result fits in a 32-bit integer. If floats are used, the expression may not yield a 32-bit integer.
>
> Design an efficient algorithm for computing $\binom{n}{k}$ which has the property that it never overflows if the final result fits in the integer word size.

##### Dynamic Programming:

```py
def compute_binomial_coefficient(n, k):
    def compute_x_choose_y(x, y):
        if y in (0, x):
            return 1
        
        if x_choose_y[x][y] == 0:
            without_y = compute_x_choose_y(x-1, y)
            with_y = compute_x_choose_y(x-1, y-1)
            x_choose_y[x][y] = without_y + with_y
        
        return x_choose_y[x][y]

    x_choose_y = [[0] * (k+1) for _ in range(n+1)]    
    return compute_x_choose_y(n, k)
```

My first instinct was try to factor $\binom{n}{k}$ in terms of common factors in the numerator and denominator; however that approach is unsatisfactory because the problem of factoring numbers itself is rather challenging.

A better approach is to consider what the binomial coefficient means fundamentally. Consider the $\small n$th element in the initial set. A subset of size $\small k$ either contains this element, or doesn't. This is the basis for a recursive enumeration - find all subsets of size $\small k-1$ amongst the first $\small n-1$ elements and add the $\small n$th element into those sets, and then find all subsets of size $\small k$ amongst the first $\small n-1$ elements. The union of these two subsets if all subsets of size $\small k$. Thus, we have our recursive sequence:


$
\binom{n}{k} = \binom{n-1}{k} + \binom{n-1}{k-1}
$


The base cases are $\binom{r}{r}$ and $\binom{r}{0}$, both of which are 1. The individual results from the subcalls are 32-bit integers and if $\binom{n}{k}$ can b represented by a 32-bit integer, they can too, so it's not possible for intermediate results to overflow.

The time and space complexity are bounded by $\small \mathcal O(nk)$, which are all the possible combinations of $\small n,k$. However, we can reduce space complexity to $\small \mathcal O(k)$, since we only use two rows at a time. 


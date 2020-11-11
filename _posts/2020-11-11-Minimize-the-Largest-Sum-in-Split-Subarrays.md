---
title: "Minimize the Largest Sum in Split Subarrays"
tags: [IT, Algorithm]
---

Intrigued by [an interesting problem](https://leetcode.com/problems/split-array-largest-sum/) as following, I'd like to share some of my thoughts toward it. Hope it can be helpful.

> Given an integer array `arr` to be divided into $ m $ non-empty continuous subarrays, minimize the largest sum among these subarrays.

## Introduction
As you can see, the original problem assumes a strong constraint toward the range of each value in the array. Many have put forward greedy solutions performing binary search between the minimum and maximum of possible largest sums, which scan the array in one pass every time we check a possible value and thus result in a time complexity $ O(n\log(sum)) $ ($ n $ is the length of array). This is efficient enough since $ f(x) = \log(x) $ has a much smaller derivative compared to $ f(x) = x $, providing a good performance in most cases even when sum is a somewhat large number.

Some others think of a more 'general' DP solution with time complexity $ O(mn^2) $. This is not so tricky as the greedy solution above, and applies to a broader range of problems. However, $ O(mn^2) $ is by no means satisfactory.

Is there another approach independent of the sum of array while achieving a relatively better time complexity?

## Intuition
### Beyond Bottom-up DP
Let's start from the DP solutions. Most of them are implemented bottom-up. One significant defect of bottom-up DP is that redundant subproblems are computed. In many cases, we only need part of them to solve the original problem. This usually only causes a difference in constant factor though.

### Interesting Observations
Define the subroutine solving the problem above as `splitArray(arr, m)`.

#### First Observation
Observe that if `arr_1` is a subarray of `arr_2`, then `splitArray(arr_1, m)` $<$ `splitArray(arr_2, m)` holds for any $ m $. This can be simply proved because the minimum of largest subarray sum cannot be larger when any subset of elements in `arr_2` is removed.

##### *Lemma 1*
If `arr_1` $ \subseteq $ `arr_2`, then `splitArray(arr_1, m)` $ \le $ `splitArray(arr_2, m)`.

#### Second Observation
Another interesting fact: suppose we divide the array `arr` into two parts, the left part `arr_left` comprising $ k $ subarrays and the right part `arr_right` comprising $ m - k $ subarrays, then `res` is the result of `splitArray(arr, m)` iff `res` is the result of $\min \\{ \max ($`splitArray(arr_left, k)`, `splitArray(arr_right, m - k)`$\\}$ in all possible divisions.

As each divison of $ m $ subarrays can be mapped to a combination of $ k $ subarrays on the left and $ m - k $ subarrays on the right (called [**bijection**](https://en.wikipedia.org/wiki/Bijection)), it can be easily shown that once either of `splitArray(arr, m)` and $\min \\{\max($`splitArray(arr_left, k)`, `splitArray(arr_right, m - k)`$)\\}$ gives a smaller value, it will break the optimality of the other.

##### *Lemma 2*
Suppose that a division $ d \in D $ split the array into `arr_left` and `arr_right` (that is, `arr` $ = $ `arr_left` $ \cup $ `arr_right`), then
`splitArray(arr, m)` $\iff \min_{d \in D} \\{ \max($`splitArray(arr_left, k)`, `splitArray(arr_right, m - k)`$)\\}$

### Combine the Lemmas
From *lemma 2*, it is enticing to adopt a *Divide & Conquer* approach:
> Choose any legit `k`, then search $\min_{d \in D} \\{ \max($`splitArray(arr_left, k)`, `splitArray(arr_right, m - k)`$)\\}$ in a total of $ n - m + 1 $ divisions.

But wait! Recall and apply *lemma 1*, we have
> For a fixed $ k $, as the division point moves rightward, the left part `splitArray(arr_left, k)` is monotonically increasing, and the right part `splitArray(arr_right, m - k)` is monotonically decreasing.

So there is **NO** need to search all of the $ n - m + 1 $ divisions! If we aim to find the division $ d $ that minimizes $  \max( $`splitArray(arr_left, k)`, `splitArray(arr_right, m - k)`$ ) $, just check the divison point at which the left and right parts have a minimized absolute value of difference.

To find the division point, employ binary search in logarithmic time. (And the search space shrinks to a logarithmic one too!)

## Solution
We have broken the problem into logarithmic subproblems now. A confusing point is how to choose the $ k $. In fact, it's all up to you. Any legit $ k $ ($ 1 \le k < m $) is acceptable. For convenience, I'll pick $ k = 1 $ for the left and hence $ m - 1 $ for the right part.

### Back to DP
So far we've been exploring D&C and binary search. But how about the aforementioned bottom-up DP?

Think of `splitArray(arr_left, 1)` and `splitArray(arr_right, m - 1)`. The left part is easy to evaluate by just calculating the sum of `arr_left` with a prefix sum array in $ O(1) $ time. But the right part will invoke subroutine `splitArray` recursively. To avoid redundant computations, we can store the result every time we execute `splitArray` indexed by its two arguments `(left bound of arr_right, m)` (as right bound will always be the same as the original array).

This is the so-called *memoization* technique, aka top-down DP.

## Time Complexity Analysis
The complete solution has been constructed now. Last, but by no means the least, is the analysis toward its time complexity.

### Recurrence Relation
Again, let's get back to the subproblem breaking step:
> `splitArray(arr, m)` $\to \min_{d \in D} \\{ \max($`splitArray(arr_left, 1)`, `splitArray(arr_right, m - 1)`$)\\}$, where `splitArray(arr_left, 1)` can be computed in $ O(1) $ time and at most $ O(\log n) $ subproblems (i.e. `splitArray(arr_right, m - 1)`) need to be searched when using binary search.

Define the time complexity of solving the original problem with the array length being $ n $ and number of subarrays being $ m $ as $ T(n, m) $. Then we have the following recurrence relation equation:

$$ T(n, m) = \sum_{i=1}^{\lceil{\log n}\rceil} T(\frac{n}{2^i}, m - 1) + O(\log n) $$

Unfortunately, it's hard to derive an analytical solution from it. But we can get an upper bound instead of an asymtotically tight one (as [big O notation](https://en.wikipedia.org/wiki/Big_O_notation) defines).

### Loose Upper Bound
Note that $ T(n, m - 1) \le T(n, m) $ as $ T(n, m) $ can be broken into $ T(n, m - 1) $ and $ T(0, 1) $ and thus subproblem $ Q(n, m - 1) $ can be reduced to $ Q(n, m) $.

Therefore,

$$ 
    T(n, m) \le \sum_{i=1}^{\lceil{\log n}\rceil} T(\frac{n}{2^i}, m) + O(\log n)
$$

$ m $ can be regarded as a constant and removed now. Simplify $ T(n, m) $ as $ T(n) $, the equation can be rewritten in

$$ T(n) \le \sum_{i=1}^{\lceil{\log n}\rceil} T(\frac{n}{2^i}) + O(\log n) $$

It's very similar to the required form of [*master theorem*](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)).

$$ T(n) \le \lceil{\log n}\rceil \cdot T(\frac{n}{2}) + O(\log n) $$

Well, it's not deducible from *master theorem* for its non-constant coefficient. To remove the $ \log n $ coefficient, transform it into another variable $ k $ denoting $ \lceil\log n\rceil $.

$$ \begin{aligned}
T(2^k) &\le kT(2^{k - 1}) + O(k) \\
&\le k \cdot ((k - 1) \cdot T(2^{k - 2}) + O(k - 1)) + O(k) \\
&\le k(k-1) \cdot T(2^{k - 2}) + O(k^2) \\
&\dotsb \\
&\le k! \cdot T(1) + O(k^{k}) \\
&=  O(k!) + O(k^k) \\ 
\end{aligned}
$$

Since $ k! $ is asymtotically equal to $ \sqrt{2\pi k}(\dfrac{k}{e})^k $ by [Stirling's approximation](https://en.wikipedia.org/wiki/Stirling%27s_approximation),

$$ T(2^k) \le O(k^k(\frac{\sqrt{k}}{e^k} + 1)) = O(k^k) $$

Finally convert $ k $ back to $ n $, we get

$$ T(n) \le O({\log n}^{\log n}) $$

### Linear Complexity
Compare it with the approach using $ sum $.

$$
\begin{aligned}
\log O({\log n}^{\log n}) &= O(\log n \log\log n) \\
\log O(n\log sum) &= O(\log n + \log\log sum)
\end{aligned}
$$

So it depends on the value of $ n $ and $ sum $. But it's only a loose bound, and it can be tighter.

At first we have

$$ T(n) \le \sum_{i=1}^{\lceil{\log n}\rceil} T(\frac{n}{2^i}) + O(\log n) $$

Note that $ T(\dfrac{n}{2^i}) $ has been magnified. If we try expanding the expression, 

$$
\begin{aligned}
T(n) &\le (T(\frac{n}{2}) + T(\frac{n}{2^2}) + \dotsb + T(1)) + O(\log n) \\
&\le (T(\frac{n}{2}) + O(1)) + (T(\frac{n}{2^2}) + O(1)) + \dotsb + (T(1) + O(1))) \\
& = T(\frac{n}{2}) + T(\frac{n}{2^2}) + \dotsb + T(1)
\end{aligned}
$$

As $ T(n) $ is obviously non-trivial and is at least more complex than $ O(1) $.

Observe that

$$
\begin{aligned}
T(1) &\le O(1) \\
T(2) &\le T(1) = O(1) \\
T(4) &\le T(2) + T(1) \le 2\cdot O(1) = O(2) \\
T(8) &\le T(4) + T(2) + T(1) \le O(4) \\
&\dotsm
\end{aligned}
$$

So we assume that $ T(n) \le O(\dfrac{n}2) = O(n) $. It can be proved by [mathematical induction](https://en.wikipedia.org/wiki/Mathematical_induction) technique.

Suppose $ n = 2^k $ here for convenience. For $ k = 0 $, $ T(1) = O(1) $ holds trivially. If $ T(2^i) \le O(2^i) $ holds for all $ 0 \le i \le k, i \in \mathbb{N} $

$$
\begin{aligned}
T(2^{k + 1}) &\le \sum_{i=0}^k T(2^{i}) \\
&\le \sum_{i=0}^k O(2^i) \\
&= O(\sum_{i=0}^k 2^i) \\
&= O(2^{k + 1})
\end{aligned}
$$

Therefore $ T(n) \le O(n) $ is true.

## Appendix
A Java implementation.

```java
// Prefix sum array
int[] sum;
// Memoization
int[][] memo;

public int splitArray(int[] arr, int m) {
    // Initialize prefix sum array
    sum = new int[arr.length + 1];
    sum[0] = 0;
    for (int i = 0; i < arr.length; i++) {
        sum[i + 1] = sum[i] + arr[i];
    }

    // Initialize values in memo as -1
    memo = new int[arr.length][m + 1];
    for (int i = 0; i < memo.length; i++) {
        Arrays.fill(memo[i], -1);
    }

    return splitArray(arr, 0, m);
}

/*
 * Param:
 * - arr: original array
 * - left: left boundary of the array to be split (i.e. arr[left..-1])
 * - m: number of subarrays to be split
 * Return:
 *   the minimum of largest sum in all the subarrays
 */
private int splitArray(int[] arr, int left, int m) {
    if (m == 1) return sum[arr.length] - sum[left];
    if (memo[left][m] >= 0) return memo[left][m];

    /*
     * Left array should have at least 1 element, and right array 
     * should have at least m - 1 elements to be divided into m - 1 subarrays.
     * Division point is defined as the index of the first element of right array,
     * and can only be selected between leftVal and rightVal via binary search.
     *
     * leftVal - inclusive, rightVal - exclusive
     */
    int leftVal = left + 1,
        rightVal = arr.length - m + 2,
        diff = 0;
    while (leftVal < rightVal) {
        int mid = (leftVal + rightVal) / 2;
        // Note that the result of left array can be computed in O(1) immediately
        diff = (sum[mid] - sum[left]) - splitArray(arr, mid, m - 1);

        if (diff < 0) {
            leftVal = mid + 1;
        } else {
            rightVal = mid;
        }
    }

    int res = 0;
    if (diff == 0) {
        // Left and right arrays have equal results, so either is okay
        res = sum[leftVal] - sum[left];
    } else {
        // There're no exact equal results of left and right arrays
        if (leftVal == left + 1) {
            // When binary search hits the left boundary,
            // left array always has a larger value
            res = sum[leftVal] - sum[left];
        } else if (leftVal == arr.length - m + 2) {
            // When binary search hits the right boundary,
            // right array always has a larger value
            res = splitArray(arr, leftVal - 1, m - 1);
        } else {
            // Compare when the dividing just results in a larger result
            // of left or right array and select the smaller one
            res = Math.min(sum[leftVal] - sum[left], splitArray(arr, leftVal - 1, m - 1));
        }
    }

    memo[left][m] = res;

    return res;
}
```

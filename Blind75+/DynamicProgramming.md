## Dynamic Programming

In this pattern, we will go through a set of problems to develop an understanding of DP. We will always start with a brute-force recursive solution to see the overlapping subproblems, i.e., realizing that we are solving the same problems repeatedly.

After the recursive solution, we will modify our algorithm to apply advanced techniques of Memoization and Bottom-Up Dynamic Programming to develop a complete understanding of this pattern.

Read this file first:
[DP Comprehensive Introduction](http://htmlpreview.github.io/?https://github.com/LordMoMA/Cracking-Coding-Questions-by-Patterns/blob/main/Blind75%2B/files/DP.html)

[Climbing Stairs - Dynamic Programming - Leetcode 70](https://www.youtube.com/watch?v=Y0lT9Fck7qI&t=731s&ab_channel=NeetCode)

[70. Climbing Stairs](http://www.leetcode.com/problems/climbing-stairs/)

```go
func climbStairs(n int) int {
    one := 1
    two := 1
    for i := 0; i < n - 1; i++ {
        temp := one
        one = one + two
        two = temp
    }
    return one
}
```

The expression dp[i] = dp[i-1] + dp[i-2] is a recurrence relation that forms the basis of the solution to the Climbing Stairs problem. It's derived from the problem statement itself.

The problem states that you can climb to a step in one of two ways:

Taking a single step from the previous step.
Taking two steps from the step before the previous one.
Therefore, the total number of ways to reach a step i is the sum of:

The number of ways to reach the previous step (dp[i-1]), because from there you can take a single step to reach step i.
The number of ways to reach the step before the previous one (dp[i-2]), because from there you can take two steps to reach step i.

```go
func climbStairs(n int) int {
    if n <= 2 {
        return n
    }
    dp := make([]int, n+1)
    dp[1] = 1
    dp[2] = 2
    for i := 3; i <= n; i++ {
        dp[i] = dp[i-1]+ dp[i-2]
    }
    return dp[n]
}
```

[198. House Robber](http://www.leetcode.com/problems/house-robber/)

```go

```

[213. House Robber II](http://www.leetcode.com/problems/house-robber-ii/)

```go

```

[337. House Robber III](http://www.leetcode.com/problems/house-robber-iii/)

```go

```

[152. Maximum Product Subarray](http://www.leetcode.com/problems/maximum-product-subarray/)

```go

```

[322. Coin Change](http://www.leetcode.com/problems/coin-change/)

```go

```

[518. Coin Change 2](http://www.leetcode.com/problems/coin-change-2/)

```go

```

[139. Word Break](http://www.leetcode.com/problems/word-break/)

```go

```

[140. Word Break II](http://www.leetcode.com/problems/word-break-ii/)

```go

```

[62. Unique Paths](http://www.leetcode.com/problems/unique-paths/)

```go

```

[63. Unique Paths II](http://www.leetcode.com/problems/unique-paths-ii/)

```go

```

[64. Minimum Path Sum](http://www.leetcode.com/problems/minimum-path-sum/)

```go

```

[72. Edit Distance](http://www.leetcode.com/problems/edit-distance/)

```go

```

[650. 2 Keys Keyboard](http://www.leetcode.com/problems/2-keys-keyboard/)

```go

```

[10. Regular Expression Matching](http://www.leetcode.com/problems/regular-expression-matching/)

```go

```

[44. Wildcard Matching](http://www.leetcode.com/problems/wildcard-matching/)

```go

```

[115. Distinct Subsequences](http://www.leetcode.com/problems/distinct-subsequences/)

```go

```

[97. Interleaving String](http://www.leetcode.com/problems/interleaving-string/)

```go

```

[72. Edit Distance](http://www.leetcode.com/problems/edit-distance/)

```go

```

[650. 2 Keys Keyboard](http://www.leetcode.com/problems/2-keys-keyboard/)

```go

```

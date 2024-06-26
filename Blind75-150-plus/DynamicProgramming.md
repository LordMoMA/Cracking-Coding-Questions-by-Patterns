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

[118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/description/)

```go
func generate(n int) [][]int {
    res := make([][]int, n)

    for i := range res {
        res[i] = make([]int, i+1)
        res[i][0], res[i][i] = 1, 1
        for j := 1; j < i; j++ {
            res[i][j] = res[i-1][j-1] + res[i-1][j]
        }
    }
    return res
}
```

[198. House Robber](http://www.leetcode.com/problems/house-robber/)

Less Memory usage:

```go
func rob(nums []int) int {
    return robRange(nums, 0, len(nums)-1)
}

func robRange(nums []int, start, end int) int {
    prev, curr := 0, 0
    for i := start; i <= end; i++ {
        newCurr := max(curr, prev+nums[i])
        prev, curr = curr, newCurr
    }
    return curr
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

```go
func rob(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    if len(nums) == 1 {
        return nums[0]
    }
    if len(nums) == 2 {
        return max(nums[0], nums[1])
    }

    dp := make([]int, len(nums))
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])

    for i := 2; i < len(nums); i++ {
        dp[i] = max(dp[i-1], dp[i-2]+nums[i])
    }

    return dp[len(nums)-1]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

[213. House Robber II](http://www.leetcode.com/problems/house-robber-ii/)

```go
func rob(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }
    if n == 1 {
        return nums[0]
    }
    return max(robRange(nums, 0, n-2), robRange(nums, 1, n-1))
}

func robRange(nums []int, start, end int) int {
    prev, curr := 0, 0
    for i := start; i <= end; i++ {
        newCurr := max(curr, prev+nums[i])
        prev, curr = curr, newCurr
    }
    return curr
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

[91. Decode Ways](http://www.leetcode.com/problems/decode-ways/)
let's illustrate the dynamic programming (DP) solution for the "91. Decode Ways" problem with the string "123".

We initialize a DP array of size n+1 (where n is the length of the string) as follows:

```go
dp = [1, 1, 0, 0]
```

The first two elements are set to 1 because there is one way to decode an empty string and one way to decode a string of length 1 if it's not '0'.

We then iterate over the string from the second character onwards and update the DP array as follows:

If the current character is not '0', it can be decoded on its own, so we add the number of ways to decode the string up to the previous index to the current index. In this case, the second character is '2', so we add dp[1] to dp[2], resulting in dp = [1, 1, 1, 0].

If the current character and the previous character form a valid two-digit number (10 to 26), it can be decoded as a pair, so we add the number of ways to decode the string up to the second previous index to the current index. In this case, '12' is a valid two-digit number, so we add dp[0] to dp[2], resulting in dp = [1, 1, 2, 0].

We repeat this process for the third character, '3'. Since '3' is not '0', we add dp[2] to dp[3], resulting in dp = [1, 1, 2, 2]. Since '23' is a valid two-digit number, we also add dp[1] to dp[3], resulting in dp = [1, 1, 2, 3].

So, there are 3 ways to decode the string "123", which is the last element in the DP array.

```go
func numDecodings(s string) int {
    if s[0] == '0' {
        return 0
    }

    dp := make([]int, len(s)+1)
    dp[0] = 1
    dp[1] = 1

    for i := 2; i <= len(s); i++ {
        if s[i-1] != '0' {  // attention: it's s[i-1], not dp[i-1]
            dp[i] += dp[i-1]
        }
        // attention: it's s[i-2], not dp[i-2]
        if s[i-2] == '1' || (s[i-2] == '2' && s[i-1] <=  '6') {
            dp[i] += dp[i-2]
        }
    }

    return dp[len(s)]
}
```

[322. Coin Change](http://www.leetcode.com/problems/coin-change/)

let's break down dp[x] = min(dp[x], dp[x-coin]+1).

This line of code is trying to find the minimum number of coins needed to make up the amount x. There are two options:

dp[x]: This is the current minimum number of coins needed to make up the amount x. It's the result we've calculated in previous steps.

dp[x-coin]+1: This represents the scenario where we use one coin of value coin to contribute towards the total amount x. So, we look at the minimum number of coins needed to make up the amount x-coin (which we've already calculated), and add one (for the coin we're currently considering).

Let's consider an example where coins = [1, 2, 5] and amount = 11.

When x = 11 and coin = 5, dp[11-5] or dp[6] is the minimum number of coins needed to make up the amount 6. If we add one 5-coin to this, we get a way to make up the amount 11. So, dp[6]+1 is a candidate for the minimum number of coins needed to make up the amount 11.

We then take the minimum of dp[11] (the current minimum number of coins for amount 11) and dp[6]+1 (the new candidate), and store it in dp[11]. This ensures that dp[11] always contains the minimum number of coins needed to make up the amount 11.

The initialization dp[i] = amount + 1 is a common technique used in dynamic programming problems where the goal is to find the minimum of something

If we initialized dp[i] to amount or any value less than amount + 1, there could be cases where the minimum number of coins needed is actually equal to amount, and we wouldn't be able to distinguish this valid case from our initial value.

In other words, amount + 1 serves as a placeholder for "infinity" to indicate that we haven't found a way to make up i with the given coins yet. If dp[i] remains amount + 1 after our algorithm, it means i cannot be made up by any combination of the given coins.

Let's illustrate the bottom-up dynamic programming solution for the coin change problem with an example.

Consider coins = [1, 2, 5] and amount = 11.

Initialize the dp array with size amount+1 (12 in this case). Set all elements to amount+1 (12), except dp[0], which is set to 0.

dp = [0, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12]

For each coin, iterate through the dp array from the index of coin to amount.

For coin = 1, update dp[x] for x from 1 to 11. After this step, dp looks like this:

dp = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

For coin = 2, update dp[x] for x from 2 to 11. After this step, dp looks like this:

dp = [0, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6]

For coin = 5, update dp[x] for x from 5 to 11. After this step, dp looks like this:

dp = [0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3]

Finally, if dp[amount] is still amount+1 (12), it means there's no way to make up the amount with the given coins, so return -1. Otherwise, return dp[amount].

In this case, dp[11] is 3, so we return 3.

This solution works by building up the solution from smaller subproblems (hence "bottom-up"). For each coin, it updates the minimum number of coins needed for all amounts that can be made up with this coin. The final answer is built up from these intermediate results.

```go
func coinChange(coins []int, amount int) int {
    dp := make([]int, amount+1)
    for i := range dp {
        dp[i] = amount + 1
    }
    dp[0] = 0

    for _, coin := range coins {
        for x := coin; x <= amount; x++ {
            dp[x] = min(dp[x], dp[x-coin]+1)
        }
    }

    if dp[amount] > amount {
        return -1
    }

    return dp[amount]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

```go
import "math"

func coinChange(coins []int, amount int) int {
    if amount == 0 {
        return 0
    }

    queue := []int{amount}
    visited := make([]bool, amount+1)
    visited[amount] = true
    level := 0

    for len(queue) > 0 {
        level++
        size := len(queue)
        for i := 0; i < size; i++ {
            remaining := queue[0]
            queue = queue[1:]
            for _, coin := range coins {
                if remaining == coin {
                    return level
                } else if remaining > coin && !visited[remaining-coin] {
                    queue = append(queue, remaining-coin)
                    visited[remaining-coin] = true
                }
            }
        }
    }

    return -1
}
```

Time Complexity: O(amount * n), where n is the number of coins. This is because for each coin, we traverse through the dp array up to amount.

Space Complexity: O(amount), as we use a dp array of length amount + 1.

Breadth-First Search (BFS) Solution:

Time Complexity: O(amount * n), where n is the number of coins. This is because in the worst case, we may need to explore each combination of coins.

Space Complexity: O(amount), as in the worst case we may need to store all amounts in the queue. The visited array also takes O(amount) space.

Note: The BFS solution's time complexity can be much worse than the DP solution in certain cases, especially when the amount is large and the number of coins is small. The BFS solution explores all possible combinations, while the DP solution reuses previously computed results.


[337. House Robber III](http://www.leetcode.com/problems/house-robber-iii/)

```go

```

[152. Maximum Product Subarray](http://www.leetcode.com/problems/maximum-product-subarray/)

Time and Space Complexity: O(n) O(1)

```go
func maxProduct(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    maxSoFar := nums[0]
    minSoFar := nums[0]
    result := maxSoFar

    for i := 1; i < len(nums); i++ {
        curr := nums[i]
        tempMax := max(curr, max(maxSoFar*curr, minSoFar*curr))
        minSoFar = min(curr, min(maxSoFar*curr, minSoFar*curr))

        maxSoFar = tempMax

        result = max(maxSoFar, result)
    }

    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

```go
func maxProduct(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    maxSoFar := nums[0]
    minSoFar := nums[0]
    result := maxSoFar

    for i := 1; i < len(nums); i++ {
        curr := nums[i]
        if curr < 0 {
            // Swap maxSoFar and minSoFar when current number is negative
            maxSoFar, minSoFar = minSoFar, maxSoFar
        }
        maxSoFar = max(curr, maxSoFar*curr)
        minSoFar = min(curr, minSoFar*curr)

        result = max(result, maxSoFar)
    }

    return result
}

```

[139. Word Break](https://leetcode.com/problems/word-break/description/)

Time: O(n^2) Space O(m+n)

```go
func wordBreak(s string, wordDict []string) bool {
    wordSet := make(map[string]bool)
    for _, word := range wordDict {
        wordSet[word] = true
    }

    dp := make([]bool, len(s)+1)
    dp[0] = true

    for i := 1; i <= len(s); i++ {
        for j := 0; j < i; j++ {
            if dp[j] && wordSet[s[j:i]] {
                dp[i] = true
                break
            }
        }
    }

    return dp[len(s)]
}
```

dp[j]: This checks if the substring s[0...j] can be segmented into a space-separated sequence of one or more dictionary words. If dp[j] is true, it means s[0...j] is a valid sequence of words.

wordSet[s[j:i]]: This checks if the substring s[j...i] is a word in the dictionary. If wordSet[s[j:i]] is true, it means s[j...i] is a valid word.

If both conditions are true, it means that the substring s[0...i] can be segmented into a space-separated sequence of one or more dictionary words, so we set dp[i] as true.

[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

Solved in DP, could also be solved in Binary Search Solution

DP: O(n^2) time complexity and O(n) space complexity

```go
func lengthOfLIS(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    dp := make([]int, len(nums))
    for i := range dp {
        dp[i] = 1
    }

    for i := range nums {
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                dp[i] = max(dp[i], dp[j]+1)
            }
        }
    }

    return max(dp...)
}

func max(nums ...int) int {
    maxNum := nums[0]
    for _, num := range nums {
        if num > maxNum {
            maxNum = num
        }
    }
    return maxNum
}
```

[62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)

let's illustrate the uniquePaths function with an example. Suppose we have m = 3 and n = 3, which means we have a 3x3 grid.

Initialization: We first initialize a 3x3 DP array with all elements as 0.

Base Case Initialization: We set dp[i][0] and dp[0][j] to 1 for all valid i and j.

State Transition: For each cell (i, j), we set dp[i][j] to dp[i-1][j] + dp[i][j-1].

For (i, j) = (1, 1), dp[1][1] = dp[0][1] + dp[1][0] = 1 + 1 = 2.
For (i, j) = (1, 2), dp[1][2] = dp[0][2] + dp[1][1] = 1 + 2 = 3.
For (i, j) = (2, 1), dp[2][1] = dp[1][1] + dp[2][0] = 2 + 1 = 3.
For (i, j) = (2, 2), dp[2][2] = dp[1][2] + dp[2][1] = 3 + 3 = 6.
After these calculations, dp becomes:

Final Answer: The number of unique paths to reach the bottom-right cell (2, 2) is dp[2][2] = 6.

So, the 2D DP array dp stores the number of unique paths to reach each cell from the top-left cell. The final answer is the number of unique paths to reach the bottom-right cell, which is stored in dp[m-1][n-1].

time and space: O(m*n)

```go
func uniquePaths(m int, n int) int {
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }

    for i := 0; i < m; i++ {
        dp[i][0] = 1
    }

    for j := 0; j < n; j++ {
        dp[0][j] = 1
    }

    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
        }
    }

    return dp[m-1][n-1]
}
```

[1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description/)

The reason for using m+1 and n+1 when creating the DP array in the Longest Common Subsequence problem is to account for the base case where one or both of the strings are empty.

In this problem, dp[i][j] represents the length of the longest common subsequence between the first i characters of text1 and the first j characters of text2. When i or j is 0, it means we're considering an empty string, because there are 0 characters from text1 or text2. Therefore, we need an extra row and an extra column in the DP array to represent these base cases.

In other problems, whether you need to add 1 when creating the DP array depends on how you define the states and the base cases. If the base case involves a 0 index (like an empty string or an array of size 0), then you'll likely need to add 1 when creating the DP array.

```go
func longestCommonSubsequence(text1 string, text2 string) int {
    m, n := len(text1), len(text2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if text1[i-1] == text2[j-1] {
                dp[i][j] = dp[i-1][j-1] + 1
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            }
        }
    }

    return dp[m][n]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```
The reason for starting the loops from 1 is because we have already initialized dp[i][0] and dp[0][j] to 0 for all valid i and j in the DP table initialization step. These represent the base cases where one or both of the strings are empty.

In Go, when you create a slice (or an array) with make, all elements are automatically initialized to the zero value of the element type. For int, the zero value is 0.

So, when you do:

 ```go
 dp := make([][]int, m+1)
for i := range dp {
    dp[i] = make([]int, n+1)
}
 ```

 You're creating a 2D slice dp with m+1 rows and n+1 columns, and all elements are initialized to 0. This includes dp[i][0] for all i from 0 to m, and dp[0][j] for all j from 0 to n. These represent the base cases where one or both of the strings are empty.

If text1[i-1] is not equal to text2[j-1], it means the current characters in text1 and text2 do not match. Therefore, the LCS up to the ith character of text1 and the jth character of text2 does not include the current characters.

In this case, the LCS could either be the LCS of the first i-1 characters of text1 and the first j characters of text2 (i.e., excluding the current character from text1), or the LCS of the first i characters of text1 and the first j-1 characters of text2 (i.e., excluding the current character from text2).

[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

It's a more space-efficient version of DP known as Kadane's algorithm.

Kadane's algorithm essentially does the same thing, but it only keeps track of the current sum (currentSum, equivalent to dp[i]) and the maximum sum seen so far (maxSoFar, equivalent to the maximum value in dp). This reduces the space complexity from O(n) to O(1), but the time complexity remains O(n).

```go
func maxSubArray(nums []int) int {
    maxSoFar := nums[0]
    currentSum := nums[0]
    
    for i := 1; i < len(nums); i++ {
        // attention, it's currentSum < 0, not nums[i] < 0
        if currentSum < 0 { 
            currentSum = nums[i]
        } else {
            currentSum += nums[i]
        }
        
        if currentSum > maxSoFar {
            maxSoFar = currentSum
        }
    }
    
    return maxSoFar
}
```

Traditional DP solution:

```go
func maxSubArray(nums []int) int {
    dp := make([]int, len(nums))
    dp[0] = nums[0]
    maxSoFar := dp[0]

    for i := 1; i < len(nums); i++ {
        dp[i] = max(nums[i], nums[i] + dp[i-1])
        maxSoFar = max(maxSoFar, dp[i])
    }
    return maxSoFar
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```
[119. Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/description/)

Use breakpoints to understand this logic, especially how the inner loop for j is going.

```go
func getRow(rowIndex int) []int {
    pascal := make([]int, rowIndex+1)
    pascal[0] = 1
    for i := 1; i < rowIndex+1; i++ {
        for j := i; j > 0; j-- {
            if j == i {
                pascal[j] = 1
            } else {
                pascal[j] = pascal[j] + pascal[j-1]
            }
        }
    }
    return pascal
}
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


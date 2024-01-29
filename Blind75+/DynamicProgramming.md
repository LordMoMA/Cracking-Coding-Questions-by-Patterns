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

O(n^2) time complexity and O(n) space complexity

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

Binary Search Solution

This solution has a time complexity of O(n log n) and a space complexity of O(n), where n is the length of the input slice nums.

if nums = [10, 9, 2, 5, 3, 7, 101, 18], the code will produce tails = [2, 3, 7, 18] and return 4

let's break down how this code works with an example. Suppose we have nums = [10, 9, 2, 5, 3, 7, 101, 18] and we're iterating through nums in the lengthOfLIS function.

num = 10: tails is currently empty. sort.Search returns 0 because there's no element in tails that is greater than or equal to 10. Since 0 == len(tails), we append 10 to tails. Now, tails = [10].

num = 9: sort.Search returns 0 because 10 (the first element in tails) is greater than 9. Since 0 != len(tails), we replace tails[0] with 9. Now, tails = [9].

num = 2: sort.Search returns 0 because 9 is greater than 2. We replace tails[0] with 2. Now, tails = [2].

num = 5: sort.Search returns 1 because there's no element in tails that is greater than or equal to 5. We append 5 to tails. Now, tails = [2, 5].

num = 3: sort.Search returns 1 because 5 is greater than 3. We replace tails[1] with 3. Now, tails = [2, 3].

num = 7: sort.Search returns 2 because there's no element in tails that is greater than or equal to 7. We append 7 to tails. Now, tails = [2, 3, 7].

num = 101: sort.Search returns 3 because there's no element in tails that is greater than or equal to 101. We append 101 to tails. Now, tails = [2, 3, 7, 101].

num = 18: sort.Search returns 3 because 101 is greater than 18. We replace tails[3] with 18. Now, tails = [2, 3, 7, 18].

At the end of the process, tails contains the smallest tail elements of all increasing subsequences of lengths 1, 2, 3, 4. The length of tails is 4, which is the length of the longest increasing subsequence in nums.

```go
import "sort"

func lengthOfLIS(nums []int) int {
    tails := []int{}
    for _, num := range nums {
        i := sort.Search(len(tails), func(i int) bool { return tails[i] >= num })
        if i == len(tails) {
            tails = append(tails, num)
        } else {
            tails[i] = num
        }
    }
    return len(tails)
}
```

```go
func lengthOfLIS(nums []int) int {
    tails := []int{}
    for _, num := range nums {
        left, right := 0, len(tails)
        for left < right {
            mid := left + (right-left)/2
            if tails[mid] < num {
                left = mid + 1
            } else {
                right = mid
            }
        }
        i := left
        if i == len(tails) {
            tails = append(tails, num)
        } else {
            tails[i] = num
        }
    }
    return len(tails)
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


[5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description/)

Manacher's algorithm solves this problem by considering each character as the center of a palindrome and expanding around it to find the maximum length of the palindrome. It cleverly reuses the information from the previous expansions to avoid unnecessary computations.

Manacher's algorithm: O(n) time and O(n) space:
[Manacher's algorithm Explaination](https://books.halfrost.com/leetcode/ChapterFour/0001~0099/0005.Longest-Palindromic-Substring/)

```go
func longestPalindrome(s string) string {
    if len(s) < 2 {
        return s
    }
    newS := make([]rune, 0)
    newS = append(newS, '#')
    for _, c := range s {
        newS = append(newS, c)
        newS = append(newS, '#')
    }
    // dp[i]:    The radius of the palindrome centered at index i in the preprocessed string (excluding the center for odd lengths)
    // maxRight: The rightmost index that can be reached by expanding around the center
    // center:   The index of the center character corresponding to maxRight
    // maxLen:   The radius of the longest palindromic substring
    // begin:    The starting index of the longest palindromic substring in the original string s
    dp, maxRight, center, maxLen, begin := make([]int, len(newS)), 0, 0, 1, 0
    for i := 0; i < len(newS); i++ {
        if i < maxRight {
            // This line is the key to Manacher's algorithm
            dp[i] = min(maxRight-i, dp[2*center-i])
        }
        // Update dp[i] using the center expansion method
        left, right := i-(1+dp[i]), i+(1+dp[i])
        for left >= 0 && right < len(newS) && newS[left] == newS[right] {
            dp[i]++
            left--
            right++
        }
        // Update maxRight, it is the maximum of i + dp[i] for all traversed i's
        if i+dp[i] > maxRight {
            maxRight = i + dp[i]
            center = i
        }
        // Record the length of the longest palindromic substring and its starting point in the original string
        if dp[i] > maxLen {
            maxLen = dp[i]
            begin = (i - maxLen) / 2 // Divide by 2 here because of the auxiliary character # we inserted
        }
    }
    return s[begin : begin+maxLen]
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```

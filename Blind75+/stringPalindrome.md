[5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description/)

DP: O(n^2) time and O(n^2) space:

```go
func longestPalindrome(s string) string {
    n := len(s)
    if n < 2 {
        return s
    }

    start, maxLen := 0, 1
    dp := make([][]bool, n)
    for i := range dp {
        dp[i] = make([]bool, n)
        dp[i][i] = true
    }

    for begin := n - 1; begin >= 0; begin-- {
        for end := begin + 1; end < n; end++ {
            if s[begin] == s[end] {
                if end-begin == 1 || dp[begin+1][end-1] {
                    dp[begin][end] = true
                    if end-begin+1 > maxLen {
                        maxLen = end - begin + 1
                        start = begin
                    }
                }
            }
        }
    }

    return s[start : start+maxLen]
}
```

Expand Around Center: O(n^2) time and O(1) space:

```go
func longestPalindrome(s string) string {
    res := ""
    for i := 0; i < len(s); i ++ {
        res = maxPalindrome(s, res, i, i)
        res = maxPalindrome(s, res, i, i+1)    
    }
    return res
}

func maxPalindrome(s string, res string, l, r int) string{
    sub := ""
    for l >= 0 && r < len(s) && s[l] == s[r]{
        sub = s[l : r+1]
        l--
        r++
    }
    if len(sub) > len(res){
        return sub
    }
    return res
}
```

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

[647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/)

Pay attention to the difference of the two solutions below:

```go
func countSubstrings(s string) int {
    count := 0
    for i := 0; i < len(s); i++ {
        count += palindrome(s, i, i)
        count += palindrome(s, i, i+1)
    }
    return count
}

func palindrome(s string, l, r int) int {
    count := 0
    for l >= 0 && r < len(s) && s[l]==s[r]{
        count++
        l--
        r++
    }
    return count
}
```

```go
func countSubstrings(s string) int {
    count := 0
    for i := 0; i < len(s); i++ {
        count = palindrome(s, count, i, i)
        count = palindrome(s, count, i, i+1)
    }
    return count
}

func palindrome(s string, count, l, r int) int {
    for l >= 0 && r < len(s) && s[l]==s[r]{
        count++
        l--
        r++
    }
    return count
}
```
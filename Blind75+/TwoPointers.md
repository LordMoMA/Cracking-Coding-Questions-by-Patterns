[392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)

```go
func isSubsequence(s string, t string) bool {
    l, r := 0, 0

    for r < len(t) && l < len(s) {
        if s[l] == t[r] {
            l++
        }
        r++
    }

    return l == len(s)
}
```


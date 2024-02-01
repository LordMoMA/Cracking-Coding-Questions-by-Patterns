[14. Longest Common Prefix](http://www.leetcode.com/problems/longest-common-prefix/)

The strings.Index(s, substr) function in Go returns the index of the first instance of substr in s. If substr is not present in s, -1 is returned.

Use vertical scanning version.

```go
// Horizontal Scanning
func longestCommonPrefix(strs []string) string {
    if len(strs) == 0 {
        return ""
    }
    prefix := strs[0]
    for i := 1; i < len(strs); i++ {
        for strings.Index(strs[i], prefix) != 0 {
            prefix = prefix[:len(prefix)-1]
            if prefix == "" {
                return ""
            }
        }
    }
    return prefix
}

// 🫡 Vertical Scanning 
func longestCommonPrefix(strs []string) string {
    if len(strs) == 0 {
        return ""
    }
    for i := 0; i < len(strs[0]); i++ {
        for j := 1; j < len(strs); j++ {
            if i == len(strs[j]) || strs[j][i] != strs[0][i] {
                return strs[0][:i]
            }
        }
    }
    return strs[0]
}
```

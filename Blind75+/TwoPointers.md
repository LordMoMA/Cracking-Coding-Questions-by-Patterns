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

[58. Length of Last Word](https://leetcode.com/problems/length-of-last-word/)

```go
func lengthOfLastWord(s string) int {
    end := len(s)-1
    for end >= 0 && s[end] == ' ' {
        end--
    }
    start := end
    for start >= 0 && s[start] != ' ' {
        start--
    }
    return end - start
}

func lengthOfLastWord2(s string) int {
    s = strings.TrimRight(s, " ")
    split := strings.Split(s, " ")
    return len(split[len(split)-1])
}
```


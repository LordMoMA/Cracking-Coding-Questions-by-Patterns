[205. Isomorphic Strings](http://www.lintcode.com/en/problem/isomorphic-strings/)

```go
func isIsomorphic(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }
    m1, m2 := make(map[byte]byte), make(map[byte]byte)
    for i := 0; i < len(s); i++ {
        if m1[s[i]] == 0 {
            m1[s[i]] = t[i]
        } else if m1[s[i]] != t[i] {
            return false
        }
        if m2[t[i]] == 0 {
            m2[t[i]] = s[i]
        } else if m2[t[i]] != s[i] {
            return false
        }
    }
    return true
}
```

A more readable solution:

```go
func isIsomorphic(s string, t string) bool {
    ms := make(map[byte]byte)
    mt := make(map[byte]byte)
    for i := 0; i < len(s); i++ {
        if _, ok := ms[s[i]]; !ok {
            ms[s[i]] = t[i]
        }
        if _, ok := mt[t[i]]; !ok {
            mt[t[i]] = s[i]
        }
        if ms[s[i]] != t[i] || mt[t[i]] != s[i] {
            return false
        } 
    }
    return true
}
```


[217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/description/)

```go
func containsDuplicate(nums []int) bool {
    cache := map[int]bool{}
    for _, num := range nums {
        if cache[num] {
            return true
        }
        cache[num] = true
    }
    return false
}
```

[242. Valid Anagram](http://leetcode.com/problems/valid-anagram/description/)

```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    count := make(map[rune]int)
    for _, char := range s {
        count[char]++
    }

    for _, char := range t {
        count[char]--
        if count[char] < 0 { // the most important check
            return false
        }
    }

    return true
}
```
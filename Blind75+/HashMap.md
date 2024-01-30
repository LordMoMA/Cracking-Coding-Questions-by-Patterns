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
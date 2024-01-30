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

[1. Two Sum](https://leetcode.com/problems/two-sum/description/)

```go
func twoSum(nums []int, target int) []int {
    cache := make(map[int]int)

    for i := 0; i < len(nums); i++ {
        another := target - nums[i]
        if _, ok := cache[another]; ok {
            return []int{cache[another], i}
        }
        cache[nums[i]] = i
    }
    return nil
}
```

[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/description/)

```go
func groupAnagrams(strs []string) [][]string {
    cache := map[string][]string{}
    res := [][]string{}
    for _, str := range strs {
        sortedStr := sortString(str)
        cache[sortedStr] = append(cache[sortedStr], str)
    }

    for _, group := range cache {
        res = append(res, group)
    }

    return res
}

func sortString(str string) string {
    split := strings.Split(str, "")
    sort.Strings(split)
    return strings.Join(split, "")
}
```
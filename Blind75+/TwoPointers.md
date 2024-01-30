[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

time is O(n), space is O(1)

```go
func isPalindrome(s string) bool {
    l, r := 0, len(s)-1
    for l < r {
        for l < r && !isAlphaNumeric(s[l]) {
            l++
        }
        for l < r && !isAlphaNumeric(s[r]) {
            r--
        }
        if l < r && !isEqual(s[l], s[r]) {
            return false
        }
        l++
        r--
    }
    return true
}

func isAlphanumeric(c byte) bool {
    return ('a' <= c && c <= 'z') || ('A' <= c && c <= 'Z') || ('0' <= c && c <= '9')
}

func isAlphaNumeric2(c byte) bool {
    return isDigit(c) || isAlpha(c)
}

func isDigit(c byte) bool {
    return c >= '0' && c <= '9'
}

func isAlpha(c byte) bool {
    return (c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z')
}
```

time and space are both O(n)

```go
func isPalindrome(s string) bool {
    s = strings.ToLower(s)
    runes := []rune{}
    for _, r := range s {
        if unicode.IsLetter(r) || unicode.IsNumber(r){
            runes = append(runes, r)
        }
    }

    for i := 0; i < len(runes)/2; i++ {
        if runes[i] != runes[len(runes) - i - 1] {
            return false
        }
    }
    return true
}
```

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

[27. Remove Element](http://leetcode.com/problems/remove-element/)

```go
func removeElement(nums []int, val int) int {
    i := 0
    for j := 0; j < len(nums); j++ {
        if nums[j] != val {
            nums[i] = nums[j]
            i++
        }
    }
    return i
}
```

[167. Two Sum II - Input Array Is Sorted](http://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```go
func twoSum(numbers []int, target int) []int {
    left, right := 0, len(numbers)-1
    for left < right {
        sum := numbers[left] + numbers[right]
        if sum == target {
            return []int{left + 1, right + 1}
        } else if sum < target {
            left++
        } else {
            right--
        }
    }
    return []int{}
}
```
[42. Trapping Rain Water](http://leetcode.com/problems/trapping-rain-water/)

```go
func trap(height []int) int {
    left, right := 0, len(height)-1
    maxLeft, maxRight := 0, 0
    result := 0

    for left < right {
        if height[left] < height[right] {
            if height[left] > maxLeft {
                maxLeft = height[left]
            } else {
                result += maxLeft - height[left]
            }
            left++
        } else {
            if height[right] > maxRight {
                maxRight = height[right]
            } else {
                result += maxRight - height[right]
            }
            right--
        }
    }

    return result
}
```


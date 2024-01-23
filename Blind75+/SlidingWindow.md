[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)

```go
func minWindow(s string, t string) string {
    if len(s) < len(t) {
        return ""
    }

    // Create frequency map for t
    freqMap := make(map[rune]int)
    for _, char := range t {
        freqMap[char]++
    }

    required := len(freqMap)
    formed := 0
    windowCounts := make(map[rune]int)

    left, right := 0, 0
    minLen := int(^uint(0) >> 1) // max int value
    minLeft, minRight := 0, 0

    for right < len(s) {
        char := rune(s[right])
        windowCounts[char]++

        if _, ok := freqMap[char]; ok && windowCounts[char] == freqMap[char] {
            formed++
        }

        // Try to contract the window
        for left <= right && formed == required {
            char = rune(s[left])

            // Save the smallest window
            if right-left+1 < minLen {
                minLen = right - left + 1
                minLeft = left
                minRight = right
            }

            // Move the left pointer
            windowCounts[char]--
            if _, ok := freqMap[char]; ok && windowCounts[char] < freqMap[char] {
                formed--
            }
            left++
        }

        right++
    }

    if minLen == int(^uint(0)>>1) {
        return ""
    }
    return s[minLeft : minRight+1]
}
```

[567. Permutation in String](http://leetcode.com/problems/permutation-in-string/)

Same pattern like above:

```go
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {
        return false
    }

    // Create frequency map for s1
    freqMap := make(map[rune]int)
    for _, char := range s1 {
        freqMap[char]++
    }

    required := len(freqMap)
    formed := 0
    windowCounts := make(map[rune]int)

    left, right := 0, 0

    for right < len(s2) {
        char := rune(s2[right])
        windowCounts[char]++

        if _, ok := freqMap[char]; ok && windowCounts[char] == freqMap[char] {
            formed++
        }

        // Try to contract the window
        for left <= right && formed == required {
            char = rune(s2[left])

            // Check if the current window has all the characters of s1
            if right-left+1 == len(s1) {
                return true
            }

            // Move the left pointer
            windowCounts[char]--
            if _, ok := freqMap[char]; ok && windowCounts[char] < freqMap[char] {
                formed--
            }
            left++
        }

        right++
    }

    return false
}
```

More optimized version:

note: the negative numbers in freq help us keep track of characters that are not in s1 or are in s2 more times than they are in s1.

The freq slice could look like this at some point during the execution of the program:
```go
freq := []int{2, 1, 0, 0, 3, 0, 0, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0}
```
This represents a string where 'a' appears 2 times, 'b' appears 1 time, 'e' appears 3 times, 'i' appears 1 time, 'n' appears 2 times, and 'r' appears 1 time. All other letters do not appear in the string.

```go
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {
        return false
    }

    freq := make([]int, 26)
    for _, ch := range s1 {
        freq[ch-'a']++
    }

    left, right, count := 0, 0, len(s1)
    for right < len(s2) {
        if freq[s2[right]-'a'] > 0 {
            count--
        }
        freq[s2[right]-'a']-- // important step to understand
        right++

        if count == 0 {
            return true
        }

        if right - left == len(s1) {
            if freq[s2[left]-'a'] >= 0 {
                count++
            }
            freq[s2[left]-'a']++
            left++
        }
    }

    return false
}
```

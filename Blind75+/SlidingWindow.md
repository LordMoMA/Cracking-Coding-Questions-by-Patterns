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
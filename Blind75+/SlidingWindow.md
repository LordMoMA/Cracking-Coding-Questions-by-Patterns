## Sliding Window

the Sliding Window pattern often goes hand in hand with a map (or a set, or an array, depending on the language and the specific problem).

[3. Longest Substring Without Repeating Characters](http://leetcode.com/problems/longest-substring-without-repeating-characters/)

Why not `charMap := make(map[rune]int)` ?

In Go, a string is a sequence of bytes, but those bytes are often used to represent Unicode characters, also known as runes. A single Unicode character can be one to four bytes long.

When you index a string like s[right], you're getting the byte at that index, not necessarily the full character. If the string contains non-ASCII characters (i.e., characters that are more than one byte long), indexing the string will not give you the correct character.

By converting the byte to a rune with rune(s[right]), you're ensuring that you're working with the full Unicode character, not just a single byte. This is important if the string can contain non-ASCII characters.

```go
func lengthOfLongestSubstring(s string) int {
    if len(s) == 0 {
        return 0
    }

    charMap := make(map[rune]int)
    left, right := 0, 0
    maxLen := 0

    for right < len(s) {
        char := rune(s[right])
        charMap[char]++
        right++

        for charMap[char] > 1 {
            charMap[rune(s[left])]--
            left++
        }

        maxLen = max(maxLen, right-left)
    }

    return maxLen
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

```go
func characterReplacement(s string, k int) int {
    count := make([]int, 26)
    maxCount, maxLen, start := 0, 0, 0

    for end := 0; end < len(s); end++ {
        count[s[end] - 'A']++
        maxCount = max(maxCount, count[s[end] - 'A'])
        if end - start + 1 - maxCount > k {
            count[s[start] - 'A']--
            start++
        } else {
            maxLen = max(maxLen, end - start + 1)
        }
    }
    return maxLen
}
```

[424. Longest Repeating Character Replacement](http://leetcode.com/problems/longest-repeating-character-replacement/)

```go
func characterReplacement(s string, k int) int {
    if len(s) == 0 {
        return 0
    }

    charMap := make(map[rune]int)
    left, right := 0, 0
    maxLen := 0
    maxCount := 0

    for right < len(s) {
        char := rune(s[right])
        charMap[char]++
        maxCount = max(maxCount, charMap[char])
        right++

        for right-left-maxCount > k {
            charMap[rune(s[left])]--
            left++
        }

        maxLen = max(maxLen, right-left)
    }

    return maxLen
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

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

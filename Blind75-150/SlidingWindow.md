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

🐯 Recommend the above pattern. 

```go
func main() {
    a := [3]int{1, 2, 3}
    b := [3]int{2, 1, 3}
    c := [3]int{1, 2, 3}

    fmt.Println(a == b) // Output: false
    fmt.Println(a == c) // Output: true
}
```

```go
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {
        return false
    }

    var s1Freq [26]int
    var s2Freq [26]int

    for i := 0; i < len(s1); i++ {
        s1Freq[s1[i]-'a']++
        s2Freq[s2[i]-'a']++
    }

    for i := len(s1); i < len(s2); i++ {
        if s1Freq == s2Freq {
            return true
        }
        s2Freq[s2[i]-'a']++
        s2Freq[s2[i-len(s1)]-'a']--
    }

    return s1Freq == s2Freq
}
```

[239. Sliding Window Maximum](http://leetcode.com/problems/sliding-window-maximum/)

```go

```

[438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)

```go
func findAnagrams(s string, p string) []int {
    var result []int
    if len(p) > len(s) {
        return result
    }

    pFreq := make([]int, 26)
    for _, ch := range p {
        pFreq[ch-'a']++
    }

    left, right, count := 0, 0, len(p)
    for right < len(s) {
        if pFreq[s[right]-'a'] > 0 {
            count--
        }
        pFreq[s[right]-'a']--
        right++

        if count == 0 {
            result = append(result, left)
        }

        if right - left == len(p) {
            if pFreq[s[left]-'a'] >= 0 { // attention, should be >=
                count++
            }
            pFreq[s[left]-'a']++
            left++
        }
    }

    return result
}
```

CodeSignal Problem, not one in LeetCode has similar pattern

You are given two strings - pattern and source. The first string pattern contains only the symbols 0 and 1, and the second string source contains only lowercase English letters.

Let's say that pattern matches a substring source[l..r] of source if the following three conditions are met:

they have equal length,
for each 0 in pattern the corresponding letter in the substring is a vowel,
for each 1 in pattern the corresponding letter is a consonant.
Your task is to calculate the number of substrings of source that match pattern.

Note: In this task we define the vowels as 'a', 'e', 'i', 'o', 'u', and 'y'. All other letters are consonants.

Example

For pattern = "010" and source = "amazing", the output should be solution(pattern, source) = 2.

```go
func calcCount(pattern string, source string) int {
    count := 0
    for i := 0; i <= len(source)-len(pattern); i++ {
        match := true
        for j := 0; j < len(pattern); j++ {
            if (pattern[j] == '0' && !isVowel(source[i+j])) || (pattern[j] == '1' && isVowel(source[i+j])) {
                match = false
                break
            }
        }
        if match {
            count++
        }
    }
    return count
}

func isVowel(ch byte) bool {
    return strings.ContainsRune("aeiouy", rune(ch))
}
```


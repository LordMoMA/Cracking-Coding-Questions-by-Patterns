
[238. Product of Array Except Self](http://www.leetcode.com/problems/product-of-array-except-self/)

```go
func productExceptSelf(nums []int) []int {
    length := len(nums)
    answer := make([]int, length)

    // answer[i] contains the product of all the numbers to the left of i.
    answer[0] = 1
    for i := 1; i < length; i++ {
        answer[i] = nums[i-1] * answer[i-1]
    }

    // Now for each i, calculate the product of all the numbers to the right and multiply it with the product from the first pass.
    R := 1
    for i := length - 1; i >= 0; i-- {
        answer[i] = answer[i] * R
        R *= nums[i]
    }

    return answer
}
```
[271. Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/description/)

The problem "271. Encode and Decode Strings" asks to design an algorithm to encode a list of strings to a single string, and then decode the single string back into the original list of strings.

The pattern to solve this problem is to use a delimiter that won't appear in the strings, and also encode the length of each string.

The strings.Builder type in Go is used for efficient string concatenation.

In Go, strings are immutable, which means once a string is created, it cannot be changed. So when you do string concatenation using the + operator, Go has to create a new string that is the result of the concatenation, which can be inefficient if you're doing a lot of concatenations.

strings.Builder works around this by using a byte slice ([]byte) under the hood to store the string data. Appending to a byte slice is more efficient than string concatenation because it can be done in place without creating a new slice.

So if you're doing a lot of string concatenations, it's more efficient to use strings.Builder. In the Encode function, we're concatenating the length of the string, a colon, and the string itself for each string in the slice, so using strings.Builder makes this operation more efficient.

```go
codec := Constructor()
strs := []string{"Hello", "World"}
encoded := codec.Encode(strs)
// encoded could be "5:Hello5:World"
decoded := codec.Decode(encoded)
// decoded should be ["Hello", "World"]
```

"Codec" is a term that's short for "Coder-Decoder".

```go
import (
    "strconv"
    "strings"
)

type Codec struct{}

func Constructor() Codec {
    return Codec{}
}

// Encodes a list of strings to a single string.
func (this *Codec) Encode(strs []string) string {
    var sb strings.Builder
    for _, str := range strs {
        sb.WriteString(strconv.Itoa(len(str)) + ":" + str)
    }
    return sb.String()
}

// Decodes a single string to a list of strings.
func (this *Codec) Decode(s string) []string {
    i := 0
    res := []string{}
    for i < len(s) {
        colon := strings.Index(s[i:], ":")
        length, _ := strconv.Atoi(s[i : i+colon])
        i += colon + 1
        res = append(res, s[i:i+length])
        i += length
    }
    return res
}
```
There are other ways to encode a list of strings to a single string without using strings.Builder. One common alternative is to use string concatenation with the + operator. However, keep in mind that this can be less efficient than using strings.Builder if you're concatenating a large number of strings.

```go
// Encodes a list of strings to a single string.
func (this *Codec) Encode(strs []string) string {
    encoded := ""
    for _, str := range strs {
        encoded += strconv.Itoa(len(str)) + ":" + str
    }
    return encoded
}
```

Another alternative is to use the fmt.Sprintf function, which can be more readable but also less efficient:

```go
// Encodes a list of strings to a single string.
func (this *Codec) Encode(strs []string) string {
    encoded := ""
    for _, str := range strs {
        encoded += fmt.Sprintf("%d:%s", len(str), str)
    }
    return encoded
}
```

[1929. Concatenation of Array](http://www.leetcode.com/problems/concatenation-of-array/)

```go
func getConcatenation(nums []int) []int {
    return append(nums, nums...)
}

func getConcatenation2(nums []int) []int {
    n := len(nums)
    res := make([]int, 2*n)
    for i := range nums {
        res[i] = nums[i]
        res[i+n] = nums[i]
    }
    return res
}
```

[1299. Replace Elements with Greatest Element on Right Side](http://www.leetcode.com/problems/replace-elements-with-greatest-element-on-right-side/)

```go  
// O(n) time, O(1) space
func replaceElements(arr []int) []int {
    n := len(arr)
    res := make([]int, n)
    res[n-1] = -1
    for i := n-2; i >= 0; i-- {
        res[i] = max(res[i+1], arr[i+1])
    }
    return res
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

Given an array of positive integers a, your task is to calculate the sum of every possible a[i] ∘ a[j], where a[i] ∘ a[j] is the concatenation of the string representations of a[i] and a[j] respectively.

Example

For a = [10, 2], the output should be solution(a) = 1344.

a[0] ∘ a[0] = 10 ∘ 10 = 1010,
a[0] ∘ a[1] = 10 ∘ 2 = 102,
a[1] ∘ a[0] = 2 ∘ 10 = 210,
a[1] ∘ a[1] = 2 ∘ 2 = 22.
So the sum is equal to 1010 + 102 + 210 + 22 = 1344.

```go
func solution(a []int) int64 {
    var res int64
    for i := 0; i < len(a); i++ {
        for j := 0; j < len(a); j++ {
            sum := strconv.Itoa(a[i]) + strconv.Itoa(a[j])
            num, _ := strconv.ParseInt(sum, 10, 64)
            res += num
        }
    }
    return res
}

func solution2(a []int) int64 {
    var res int64
    for i := 0; i < len(a); i++ {
        for j := 0; j < len(a); j++ {
            sum := strconv.Itoa(a[i]) + strconv.Itoa(a[j])
            num, _ := strconv.Atoi(sum)
            res += int64(num)
        }
    }
    return res
}

// Pre-calculate string representations: Convert all numbers to strings at the beginning to avoid converting them multiple times in the loop.
func solution3(a []int) int64 {
    var res int64
    strNums := make([]string, len(a))
    for i, num := range a {
        strNums[i] = strconv.Itoa(num)
    }
    for _, strNum1 := range strNums {
        for _, strNum2 := range strNums {
            num, _ := strconv.ParseInt(strNum1+strNum2, 10, 64)
            res += num
        }
    }
    return res
}
```
Both above solutions: 

Tests passed: 14/16. Execution time limit exceeded on test 15: Program exceeded the execution time limit. Make sure that it completes execution in a few seconds for any possible input.
Click the "Run Tests" button to see output, console logs, and detailed error messages for sample or custom test cases. This information is hidden when clicking the "Submit" button in order to prevent hard-coding solutions to the hidden test cases.
Sample tests:
8/8
Hidden tests:
6/8
Score:
417/500

[303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/description/)

You can certainly solve the problem by looping through the array and adding up the elements between indices i and j each time the sumRange function is called. However, this approach would have a time complexity of O(n) for each sumRange query, where n is the length of the range. This could be inefficient if you have a large number of queries or large ranges.

The prefix sum approach, on the other hand, has a time complexity of O(1) for each sumRange query, after an initial preprocessing step of O(n) to compute the prefix sums. This makes it much more efficient for large numbers of queries or large ranges.

In other words, the prefix sum approach trades off a little extra space complexity and an initial preprocessing step for much faster query times. This is a common trade-off in computer science known as space-time tradeoff.

```go
package main

import "fmt"

type NumArray struct {
    prefixSum []int
}

func Constructor(nums []int) NumArray {
    prefixSum := make([]int, len(nums)+1)
    for i := 0; i < len(nums); i++ {
        prefixSum[i+1] = prefixSum[i] + nums[i]
    }
    return NumArray{prefixSum: prefixSum}
}

func (this *NumArray) SumRange(i int, j int) int {
    return this.prefixSum[j+1] - this.prefixSum[i]
}

func main() {
    nums := []int{-2, 0, 3, -5, 2, -1}
    numArray := Constructor(nums)
    fmt.Println(numArray.SumRange(0, 2)) // Outputs: 1
    fmt.Println(numArray.SumRange(2, 5)) // Outputs: -1
    fmt.Println(numArray.SumRange(0, 5)) // Outputs: -3
}
```
[448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/)

```go
func findDisappearedNumbers(nums []int) []int {
    for _, num := range nums {
        idx := abs(num) - 1
        if nums[idx] > 0 {
            nums[idx] *= -1
        }
    }
    
    res := []int{}
    for i, num := range nums {
        if num > 0 {
            res = append(res, i+1)
        }
    }
    
    return res
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

[1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/description/)

```go
func maxNumberOfBalloons(text string) int {
    count := make(map[rune]int)
    for _, ch := range text {
        count[ch]++
    }
    
    count['l'] /= 2
    count['o'] /= 2
    
    minCount := count['b']
    for _, ch := range "balon" {
        if count[ch] < minCount {
            minCount = count[ch]
        }
    }
    
    return minCount
}
```
[290. Word Pattern](https://leetcode.com/problems/word-pattern/)


```go
import (
    "strings"
)

func wordPattern(pattern string, s string) bool {
    words := strings.Split(s, " ")
    if len(pattern) != len(words) {
        return false
    }

    patternToWord := make(map[byte]string)
    wordToPattern := make(map[string]byte)

    for i := 0; i < len(pattern); i++ {
        p, w := pattern[i], words[i]
        if pw, ok := patternToWord[p]; ok && pw != w {
            return false
        }
        if wp, ok := wordToPattern[w]; ok && wp != p {
            return false
        }
        patternToWord[p] = w
        wordToPattern[w] = p
    }

    return true
}

```
[705. Design HashSet](https://leetcode.com/problems/design-hashset/description/)
```go
type MyHashSet struct {
    set map[int]bool
}

/** Initialize your data structure here. */
func Constructor() MyHashSet {
    return MyHashSet{
        set: make(map[int]bool),
    }
}

/** Adds a value to the set. */
func (this *MyHashSet) Add(key int) {
    this.set[key] = true
}

/** Removes a value from the set. */
func (this *MyHashSet) Remove(key int) {
    delete(this.set, key)
}

/** Returns true if this set contains the specified element */
func (this *MyHashSet) Contains(key int) bool {
    _, exists := this.set[key]
    return exists
}
```

[1672. Richest Customer Wealth](http://www.leetcode.com/problems/richest-customer-wealth/)

```go

```

[1920. Build Array from Permutation](http://www.leetcode.com/problems/build-array-from-permutation/)

k``go

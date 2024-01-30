
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



[1672. Richest Customer Wealth](http://www.leetcode.com/problems/richest-customer-wealth/)

```go

```

[1920. Build Array from Permutation](http://www.leetcode.com/problems/build-array-from-permutation/)

```go
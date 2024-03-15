## Reverse a String

rev[:len(sl)]: This is slicing the array rev from the start up to len(sl). This is done because rev was declared with a size of 100, but we only filled it up with len(sl) elements. So we want to ignore the extra, unused elements in rev.



```go
// variant: use of slice of byte and conversions
func reverse1(s string) string {
    sl := []byte(s)
    var rev [100]byte
    j := 0
    for i := len(sl) - 1; i >= 0; i-- {
        rev[j] = sl[i]
        j++
    }
    strRev := string(rev[:len(sl)])
    return strRev
}

// variant: "in place" using swapping _one slice
func reverse2(s string) string {
    sl2 := []byte(s)
    for i, j := 0, len(sl2)-1; i < j; i, j = i+1, j-1 {
        sl2[i], sl2[j] = sl2[j], sl2[i]
    }
    return string(sl2)
}

// variant: using [] int for runes (necessary for Unicode-strings!):
func reverse3(s string) string {
    runes := []rune(s)
    n, h := len(runes), len(runes)/2
    for i := 0; i < h; i++ {
        runes[i], runes[n-1-i] = runes[n-1-i], runes[i]
    }
    return string(runes)
}

func main() {
    // reverse a string:
    str := "Google"

    fmt.Printf("The reversed string using variant 1 is -%s-\n", reverse1(str))
    fmt.Printf("The reversed string using variant 2 is -%s-\n", reverse2(str))
    fmt.Printf("The reversed string using variant 3 is -%s-\n", reverse3(str))
}
```

## Finding number of bytes and characters in string

```go
package main

import (
  "fmt"
  "unicode/utf8"
)

func main() {
  str := "Hello, 世界"

  // Number of bytes
  byteCount := len(str)
  fmt.Println("Number of bytes:", byteCount)

  // Number of characters
  charCount := utf8.RuneCountInString(str)
  charCount2 := len([]rune(str))
  fmt.Println("Number of characters:", charCount)
  fmt.Println("Number of characters 2nd version:", charCount2)
}

// output
Number of bytes: 13
Number of characters: 9
Number of characters 2nd version: 9
```

## Concatenating strings

The fastest way is:

```go
// with a bytes.Buffer 
var buffer bytes.Buffer
var s string
buffer.WriteString(s)
fmt.Print(buffer.String(), "\n")
```

Anther fastest way:

```go
var builder strings.Builder

strs := []string{"Hello", ", ", "World", "!"}

for _, str := range strs {
    builder.WriteString(str)
}

fmt.Println(builder.String())  // Outputs: Hello, World!
```

Other ways are:

```go
Strings.Join() // using Join function
str1 += str2 // using += operator
```

bytes.Buffer and strings.Builder are both used for efficient concatenation of strings in Go, but they have some differences:

bytes.Buffer:

It can be used for both string and byte slice manipulation.
It provides methods for reading from and writing to the buffer.
It's safe for concurrent use by multiple goroutines.
strings.Builder:

It's designed specifically for building strings, so it only provides methods for writing strings.
It's simpler and safer to use than bytes.Buffer if you only need to build a string.
It's not safe for concurrent use by multiple goroutines.

In general, if you only need to build a string, strings.Builder is the better choice. If you need to manipulate byte slices or read from the buffer, you should use bytes.Buffer.
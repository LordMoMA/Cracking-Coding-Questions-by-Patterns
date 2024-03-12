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
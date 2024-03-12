## Q1

What is the output of the following Go program?

```go
package main
import "fmt"

func fp(a *[3]int) { fmt.Println(a) }

func main() {
  for i := 0; i < 3; i++ {
    fp(&[3]int{i, i * i, i * i * i}) // newly created instance
  }
}
```

```bash
&[0 0 0]

&[1 1 1]

&[2 4 8]
```
If you want to print the array without the &, you can dereference the pointer in the Println call like this:

```go
func fp(a *[3]int) { fmt.Println(*a) }
```


## Q2 

```go
b := []byte{'g', 'o', 'l', 'a', 'n', 'g'}
```
What are the length and capacity of: b[1:4] , b[:2], b[2:] and b[:] ?

In Go, slicing an array or a slice gives you a new slice that points to the same underlying array. The length of the new slice is the number of elements it includes, and its capacity is the number of elements in the underlying array from the start of the new slice.

Here's how it works for your examples:

b[1:4]: This slice includes the elements at indices 1, 2, and 3. So its length is 3. Its capacity is the number of elements from index 1 to the end of the underlying array, which is 5. So its capacity is 5.

b[:2]: This slice includes the elements at indices 0 and 1. So its length is 2. Its capacity is the number of elements from index 0 to the end of the underlying array, which is 6. So its capacity is 6.

b[2:]: This slice includes the elements from index 2 to the end of the slice. So its length is 4 (indices 2, 3, 4, 5). Its capacity is also 4, because it starts at index 2 and goes to the end of the underlying array.

b[:]: This slice includes all elements of b. So its length is 6, and its capacity is also 6, because it includes all elements of the underlying array.

## Q3

For the following code:

```go
s := make([]byte, 5)    // line 1
s = s[2:4]             // line 2
```

What are len(s) and cap(s) at line 1 and line 2 ?

At line 1, s := make([]byte, 5) creates a slice of bytes with length and capacity both equal to 5. So, len(s) is 5 and cap(s) is 5.

At line 2, s = s[2:4] reslices s to start at index 2 and end at index 4 (exclusive). This new slice includes 2 elements (at indices 2 and 3), so len(s) is 2. However, the capacity of the new slice is the number of elements from the start of the new slice (index 2) to the end of the original slice, which is 3 (indices 2, 3, and 4). So, cap(s) is 3.

## Q4

Suppose we have the following slice:

```go
items := [...]int{10, 20, 30, 40, 50}
```

If we code the following for-loop, what will be the value of items after the loop ?

```go
for _, item := range items {
    item *= 2
}
```

The items slice will remain unchanged after the loop. The reason is that the range keyword in Go returns a copy of the element, not a reference to the element. So when you do item *= 2, you're modifying a copy of the element, not the element itself in the slice.

If you want to modify the elements of the slice, you need to use the index to access and modify them:

```go
for i := range items {
    items[i] *= 2
}
```

## Q5

If s is a slice, what is the length of s[n:n] and s[n:n+1] ?

Length (len): This is the number of elements in the slice. It's determined by the difference between the end index and the start index of the slicing operation. For s[n:n], the start and end indices are the same, so there are no elements and the length is 0. For s[n:n+1], the end index is one more than the start index, so there is one element and the length is 1.

## Q6

What will the following function task return for the slice {“M”, “N”, “O”, “P”, “Q”, “R”}, index1 as 2 and index2 as 4?

```go
func task(slice []string, index1, index2 int) []string {
    result := make([]string, len(slice) - (index2 - index1))
    at := copy(result, slice[:index1])
    copy(result[at:], slice[index2:])
    return result
}

// {M N Q R}
```

The function is removing values from index1 to index2 inclusively. 
This function task removes a sub-slice from the original slice starting from index1 to index2 (exclusive) and returns the resulting slice.

Another way:

```go
func task(slice []string, index1, index2 int) []string {
    return append(slice[:index1], slice[index2:]...)
}
```

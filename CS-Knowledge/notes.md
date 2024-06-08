## On Numbers and Strings

### Why large numbers cannot be compared but strings can?

The key difference between storing numbers as integers and storing them as strings lies in how they are represented and processed in memory.

When you store a number as an integer, the computer needs to be able to perform arithmetic operations on it (like addition, subtraction, multiplication, division, etc.). To do this efficiently, the computer represents the number as a binary value. The size of the integer type (like int, long, short, etc.) determines the range of numbers it can represent. For example, a 64-bit integer can represent numbers up to 2^64 - 1. If a number exceeds this range, it can't be stored in a 64-bit integer because there aren't enough bits to represent it.

On the other hand, when you store a number as a string, each digit is stored as a separate character. The computer doesn't need to perform arithmetic operations on the characters, so it doesn't need to represent the entire number as a single binary value. This means that a string can represent a number of any size, as long as there's enough memory to store each digit as a separate character. However, if you want to perform arithmetic operations on the number represented by the string, you would need to convert it to an integer or implement the arithmetic operations yourself.

So, the reason why strings can represent larger numbers than integers is not because strings are inherently capable of storing larger values, but because they represent numbers in a different way that doesn't require the entire number to be stored as a single binary value.

### What does math/big package do under the hood?

The big.Int type from the math/big package in Go can handle arbitrarily large integers by using an array of uint values to represent the integer. Each uint value represents a "chunk" of the overall integer. This allows big.Int to represent integers that are much larger than what can be represented with a single 64-bit value.

When you perform operations on big.Int values, the math/big package performs the operations on each "chunk" of the integer separately, and then combines the results to get the final result. This is similar to how you would perform arithmetic operations on large numbers by hand, working with one digit at a time.

The big.Int type does not use string comparison under the hood. When comparing two big.Int values, the math/big package compares the "chunks" of the integers from most significant to least significant until it finds a pair of "chunks" that are not equal. The integer with the larger "chunk" is considered to be the larger integer.

So, while the big.Int type can handle larger integers than the built-in integer types, it does so by using more memory and more complex algorithms to perform arithmetic operations, not by using string comparison.

### why not using string comparision, isn't it easier to convert big int to string first and compare its length and single digit accordingly?

While it might seem easier to convert large integers to strings and compare them lexicographically, this approach has several drawbacks:

Performance: Converting a large integer to a string is a relatively expensive operation. It requires allocating a new string and then converting each digit of the integer to a character. This is much slower than comparing the integer directly.

### why not using string comparision, isn't it easier to convert big int to string first and compare its length and single digit accordingly?

While it might seem easier to convert large integers to strings and compare them lexicographically, this approach has several drawbacks:

1. Performance: Converting a large integer to a string is a relatively expensive operation. It requires allocating a new string and then converting each digit of the integer to a character. This is much slower than comparing the integer directly.

2. Memory Usage: Strings use more memory than integers. Each digit of the number requires at least one byte of memory in a string, whereas in an integer, each digit requires less than a byte (since an integer can represent a range of values in a single byte).

let's take an example. Consider the number 12345.

As an integer: In binary, 12345 is represented as 11000000111001, which is 14 bits long. On a typical system, this would be stored in a 16-bit integer type (like short in C/C++ or int16 in Go), which uses 2 bytes of memory.

As a string: The string "12345" consists of 5 characters. In most programming languages, a string is a sequence of Unicode characters. In the simplest case where each character is an ASCII character (like our digits here), each character uses 1 byte of memory. So the string "12345" uses 5 bytes of memory.

So in this example, the integer 12345 uses less memory (2 bytes) than its string representation "12345" (5 bytes).

This difference becomes even more pronounced with larger numbers. For example, the number 12345678901234567890 can be represented in 8 bytes as a 64-bit integer, but its string representation "12345678901234567890" would require 20 bytes of memory.

3. Arithmetic Operations: If you need to perform arithmetic operations (like addition, subtraction, multiplication, etc.) on the numbers, it's much more efficient to do this with integers than with strings. Performing arithmetic operations on strings requires converting the strings back to integers, performing the operation, and then converting the result back to a string.

For these reasons, it's generally more efficient to use a data type like big.Int that can handle large integers directly, rather than converting the integers to strings and using string comparison.

Memory Usage: Strings use more memory than integers. Each digit of the number requires at least one byte of memory in a string, whereas in an integer, each digit requires less than a byte (since an integer can represent a range of values in a single byte).

Arithmetic Operations: If you need to perform arithmetic operations (like addition, subtraction, multiplication, etc.) on the numbers, it's much more efficient to do this with integers than with strings. Performing arithmetic operations on strings requires converting the strings back to integers, performing the operation, and then converting the result back to a string.

For these reasons, it's generally more efficient to use a data type like big.Int that can handle large integers directly, rather than converting the integers to strings and using string comparison.

### Go's drawbacks

so priority queue in go, we can only use string comparision for large numbers, because heap package did not support math big package.

The container/heap package in Go doesn't directly support the math/big package. The heap package requires the elements of the heap to implement the sort.Interface, which includes the Len, Less, and Swap methods. The big.Int type doesn't implement these methods.

If you want to use a priority queue with large numbers, one option is to store the numbers as strings and implement the sort.Interface methods to perform lexicographic comparison. This would allow you to use the heap package to manage the priority queue.

However, keep in mind that lexicographic comparison is not the same as numeric comparison. For example, the string "200" is lexicographically less than the string "30" because '2' is less than '3'. If you want to compare the strings numerically, you would need to implement the Less method to convert the strings to big.Int values and compare them using the big.Int.Cmp method.

Here's an example of how you might do this:

```go
type BigIntHeap []string

func (h BigIntHeap) Len() int           { return len(h) }
func (h BigIntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h BigIntHeap) Less(i, j int) bool {
    a := new(big.Int)
    b := new(big.Int)
    a.SetString(h[i], 10)
    b.SetString(h[j], 10)
    return a.Cmp(b) < 0
}
func (h *BigIntHeap) Push(x interface{}) { *h = append(*h, x.(string)) }
func (h *BigIntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

the BigIntHeap type is a slice of strings that implements the sort.Interface and the heap.Interface. The Less method converts the strings to big.Int values and compares them using the big.Int.Cmp method. This allows the BigIntHeap to be used with the heap package to create a priority queue that can handle arbitrarily large numbers.

As to 

```go
func (m minHeap) Less(i, j int) bool {
    if len(m[i]) == len(m[j]) {
        return m[i] < m[j]
    }
    return len(m[i]) < len(m[j])
}
```

This function compares two strings in the heap. If the strings have the same length, it compares them lexicographically. If the strings have different lengths, it considers the shorter string to be less than the longer string.

This approach can work for comparing strings that represent non-negative integers, because a longer string represents a larger number and, for strings of the same length, lexicographic comparison is the same as numeric comparison.

However, this approach does not work for strings that represent negative integers or non-integer numbers, because the length of the string does not necessarily correspond to the magnitude of the number.

Also, keep in mind that this approach is less efficient than comparing the numbers directly, because it requires comparing the strings character by character. If you need to perform a lot of comparisons, it might be more efficient to convert the strings to big.Int values and compare them directly, as in the previous example.
Read more to expand knowledge here at [Shannon's theorem](https://en.wikipedia.org/wiki/Noisy-channel_coding_theorem#:~:text=The%20Shannon%20limit%20or%20Shannon,for%20a%20particular%20noise%20level.)

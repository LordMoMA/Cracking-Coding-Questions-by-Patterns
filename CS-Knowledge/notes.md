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

3. Arithmetic Operations: If you need to perform arithmetic operations (like addition, subtraction, multiplication, etc.) on the numbers, it's much more efficient to do this with integers than with strings. Performing arithmetic operations on strings requires converting the strings back to integers, performing the operation, and then converting the result back to a string.

For these reasons, it's generally more efficient to use a data type like big.Int that can handle large integers directly, rather than converting the integers to strings and using string comparison.

Memory Usage: Strings use more memory than integers. Each digit of the number requires at least one byte of memory in a string, whereas in an integer, each digit requires less than a byte (since an integer can represent a range of values in a single byte).

Arithmetic Operations: If you need to perform arithmetic operations (like addition, subtraction, multiplication, etc.) on the numbers, it's much more efficient to do this with integers than with strings. Performing arithmetic operations on strings requires converting the strings back to integers, performing the operation, and then converting the result back to a string.

For these reasons, it's generally more efficient to use a data type like big.Int that can handle large integers directly, rather than converting the integers to strings and using string comparison.

Read more to expand knowledge here at [Shannon's theorem](https://en.wikipedia.org/wiki/Noisy-channel_coding_theorem#:~:text=The%20Shannon%20limit%20or%20Shannon,for%20a%20particular%20noise%20level.)

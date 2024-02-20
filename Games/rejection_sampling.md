You have a function rand5() that generates a random integer from 1 to 5. Use it to write a function rand7() that generates a random integer from 1 to 7.

rand5() returns each integer with equal probability. rand7() must also return each integer with equal probability.

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

func rand5() int {
    rand.Seed(time.Now().UnixNano())
    return rand.Intn(5) + 1
}

func rand7() int {
    for {
        num := (rand5()-1)*5 + rand5()
        if num <= 21 {
            return num%7 + 1
        }
    }
}

func main() {
    fmt.Println(rand7())
}
```

you cannot use rand.Seed(time.Now()) directly in Go because rand.Seed() function expects an argument of type int64, and time.Now() returns a Time type, not an int64.

The UnixNano() method is called on time.Now() to convert the Time type to an int64 type representing the current time in nanoseconds since 1970. This int64 value is then used to seed the random number generator.

rand.Seed() is a function that initializes the default Source (the default source of uniformly-distributed pseudo-random integers) to a value that changes over time (in this case, the current time in nanoseconds). This ensures that you get a different sequence of random numbers each time you run your program.

Without this line, if you were to generate random numbers with rand.Intn(), rand.Float64(), or similar functions, you would get the same sequence of numbers every time you run your program. This is because the default seed value is 1. By seeding the random number generator with a value that changes (like the current time), you ensure that the sequence of random numbers is different each time.

num := (rand5()-1)*5 + rand5(): This line generates a random number between 1 and 25 with equal probability. It does this by treating the output of the first rand5() as the row and the output of the second rand5() as the column of a 5x5 grid. Subtracting 1 from the first rand5() and multiplying by 5 converts the row number to the appropriate starting point (0, 5, 10, 15, or 20), and the second rand5() then selects a specific column within that row. This gives you a number from 1 to 25 with equal probability.

Imagine a 5x5 grid:

```bash
1  2  3  4  5
6  7  8  9  10
11 12 13 14 15
16 17 18 19 20
21 22 23 24 25
```

Each cell in this grid represents a unique number from 1 to 25. The rows and columns are both numbered from 1 to 5.

Now, let's say rand5() generates the numbers 3 and 4. We can treat the first number as the row and the second number as the column. So, we're looking at the 3rd row and the 4th column, which corresponds to the number 14 in the grid.

The code (rand5()-1)*5 + rand5() does exactly this. Here's how:

rand5()-1: This generates a number from 0 to 4 (instead of 1 to 5). In our example, this would be 3-1 = 2.

(rand5()-1)*5: This scales the row number to the appropriate starting point in the grid. Multiplying by 5 gives us a number in the set {0, 5, 10, 15, 20}. In our example, this would be 2*5 = 10.

(rand5()-1)*5 + rand5(): This adds the column number to the scaled row number, giving us a number from 1 to 25. In our example, this would be 10 + 4 = 14.

if num <= 21: This line checks if the generated number is in the range 1-21. We reject numbers between 22 and 25 because 21 is the largest number that's divisible by 7 and less than or equal to 25. This ensures that each of the numbers 1-21 has an equal chance of being chosen, which is necessary to ensure that the final output has an equal probability of being any number between 1 and 7.

return num%7 + 1: If the generated number is in the range 1-21, we take it modulo 7, which gives a number between 0 and 6, and then add 1 to shift the range to 1-7. This is the final output of the function.

The for loop without a condition is an infinite loop in Go. It keeps running until it hits a return statement, which happens when a number in the range 1-21 is generated. This ensures that the function always returns a number, even if it has to reject several numbers first.

Python Version:

```py
import random

def rand5():
    return random.randint(1, 5)

def rand7():
    while True:
        # Generate a number from 1 to 25
        num = (rand5() - 1) * 5 + rand5()
        # If the number is in the range 1-21, take it modulo 7 and return
        if num <= 21:
            return num % 7 + 1
```

Python's random.randint() function uses the system time to seed the random number generator under the hood, which is why you don't need to manually seed it like you do in some other languages.

When you import the random module, it automatically seeds the random number generator with the current system time. This is done by calling random.seed() with no arguments, which under the hood gets the current system time via time.time() and uses that to seed the random number generator.
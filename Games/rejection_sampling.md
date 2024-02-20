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

rand.Seed() is a function that initializes the default Source (the default source of uniformly-distributed pseudo-random integers) to a value that changes over time (in this case, the current time in nanoseconds). This ensures that you get a different sequence of random numbers each time you run your program.

Without this line, if you were to generate random numbers with rand.Intn(), rand.Float64(), or similar functions, you would get the same sequence of numbers every time you run your program. This is because the default seed value is 1. By seeding the random number generator with a value that changes (like the current time), you ensure that the sequence of random numbers is different each time.

num := (rand5()-1)*5 + rand5(): This line generates a random number between 1 and 25 with equal probability. It does this by treating the output of the first rand5() as the row and the output of the second rand5() as the column of a 5x5 grid. Subtracting 1 from the first rand5() and multiplying by 5 converts the row number to the appropriate starting point (0, 5, 10, 15, or 20), and the second rand5() then selects a specific column within that row. This gives you a number from 1 to 25 with equal probability.

if num <= 21: This line checks if the generated number is in the range 1-21. We reject numbers between 22 and 25 because 21 is the largest number that's divisible by 7 and less than or equal to 25. This ensures that each of the numbers 1-21 has an equal chance of being chosen, which is necessary to ensure that the final output has an equal probability of being any number between 1 and 7.

return num%7 + 1: If the generated number is in the range 1-21, we take it modulo 7, which gives a number between 0 and 6, and then add 1 to shift the range to 1-7. This is the final output of the function.

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
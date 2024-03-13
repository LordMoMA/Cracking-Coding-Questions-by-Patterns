## Semophore Pattern

Implementing a semaphore using a buffered channel

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    sem := make(chan struct{}, 2)

    for i := 0; i < 5; i++ {
        sem <- struct{}{} // acquire semaphore
        go func(i int) {
            defer func() { <-sem }() // release semaphore
            fmt.Println(i)
            time.Sleep(1 * time.Second)
        }(i)
    }

    time.Sleep(2 * time.Second)
}
```

In this code, the semaphore pattern is used to ensure that no more than 2 of the launched goroutines are running concurrently. The sem channel is created with a buffer size of 2, which means it can hold up to 2 values without blocking.

In Go, the struct{} type is a zero-width type, meaning it doesn't consume any additional memory. It's often used in situations where you need a channel, but you don't care about the values being sent on the channel.

In this case, the semaphore doesn't need to carry any meaningful information; it's just being used to control access to a resource. Therefore, struct{} is a more efficient choice than bool because it doesn't consume any additional memory.

The struct{}{} value is sent on the sem channel to acquire the semaphore, and received from the sem channel to release the semaphore. The defer keyword ensures that the semaphore is released when the goroutine exits, even if it exits due to an error.
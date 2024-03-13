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

## Channel Factory and Producer-Consumer Pattern
    
```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    ch := make(chan int)
    var wg sync.WaitGroup
    wg.Add(1)
    go producer(ch, &wg)
    go consumer(ch)
    wg.Wait()
}

func producer(ch chan int, wg *sync.WaitGroup) {
    for i := 0; i < 10; i++ {
        ch <- i
    }
    close(ch)
    wg.Done()
}

func consumer(ch chan int) {
    for i := range ch {
        fmt.Println(i)
    }
}
```

## Producer-consumer pattern

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    ch := make(chan int)
    var wg sync.WaitGroup
    wg.Add(1)
    go producer(ch, &wg)
    go consumer(ch)
    wg.Wait()
}

func producer(ch chan int, wg *sync.WaitGroup) {
    for i := 0; i < 10; i++ {
        ch <- i
    }
    close(ch)
    wg.Done()
}

func consumer(ch chan int) {
    for i := range ch {
        fmt.Println(i)
    }
}
```

In this pattern, the producer and consumer run concurrently. The producer sends data to the channel and the consumer reads data from the channel. The WaitGroup is used to ensure that the main function doesn't exit until the producer has finished sending data. The close(ch) call in the producer function signals to the consumer that no more data will be sent.

If you don't use a WaitGroup in this scenario, your program may exit before your goroutines have a chance to finish executing. This is because the main function doesn't wait for goroutines to finish before it exits. If the main function exits, all goroutines are abruptly stopped.

In your code, the WaitGroup is used to make the main function wait until the producer goroutine has finished sending data to the channel. If you remove the WaitGroup, the main function may exit before the producer has sent all its data, and the consumer may not receive all the data.

You typically use a WaitGroup in scenarios where you need to wait for one or more goroutines to complete before continuing. This is common in scenarios where you have concurrent operations that other operations depend on. For example, if you're fetching data from multiple URLs concurrently, you might use a WaitGroup to wait until all the data has been fetched before processing it.


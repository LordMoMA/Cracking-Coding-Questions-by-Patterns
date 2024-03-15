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

## Pipe and filter pattern

The Sieve-Prime Algorithm

version 1:

```go
package main
import "fmt"

// Send the sequence 2, 3, 4, ... to channel ch.
func generate(ch chan int) {
  for i := 2; ; i++ {
    ch <- i // Send i to channel ch.
  }
}

// Copy the values from channel in to channel out,
// removing those divisible by prime.
func filter(in, out chan int, prime int) {
  for {
    i := <-in // Receive value of new variable i from in.
    if i%prime != 0 {
      out <- i // Send i to channel out.
    }
  }
}

// The prime sieve
func main() {
  ch := make(chan int) // Create a new channel.
  go generate(ch) // Start generate() as a goroutine.
  for {
    prime := <-ch
    fmt.Print(prime, " ")
    ch1 := make(chan int)
    go filter(ch, ch1, prime)
    ch = ch1
  }
}
```

version 2:

```go
package main
import "fmt"

// Send the sequence 2, 3, 4, ... to returned channel
func generate() chan int {
  ch := make(chan int)
  go func() {
    for i := 2; ; i++ {
     ch <- i
    }
  }()
  return ch
}

// Filter out input values divisible by prime, send rest to returned channel
func filter(in chan int, prime int) chan int {
  out := make(chan int)
  go func() {
    for {
      if i := <-in; i%prime != 0 {
       out <- i
      }
    }
  }()
  return out
}

func sieve() chan int {
  out := make(chan int)
  go func() {
    ch := generate()
    for {
      prime := <-ch
      ch = filter(ch, prime)
      out <- prime
    }
  }()
  return out
}

func main() {
  primes := sieve()
  for {
    fmt.Println(<-primes)
  }
}
```

### Devise Random Bit Generator

Create a random bit generator that is a program that produces a sequence of 100000 randomly generated 1’s and 0’s using a goroutine. You can use the select statement to randomly choose the cases for random generation.

```go
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int)
    // consumer:
    go func() {
        for {
            fmt.Print(<-ch, " ")
        }
    }()
    // producer:
    for i:=0; i<=100000; i++ {
        select {
            case ch <- 0: 
            case ch <- 1:
        }
    }

}
```
Problem statement
Write an interactive console program that asks the user for the polar coordinates of a 2-dimensional point (radius and angle (degrees)). Calculate the corresponding Cartesian coordinates x and y, and print out the result. Use structs called polar and Cartesian to represent each coordinate system. Use channels and a goroutine:

A channel1 to receive the polar coordinates
A channel2 to receive the Cartesian coordinates
The conversion itself must be done with a goroutine, which reads from channel1 and sends it to channel2. In reality, for such a simple calculation it is not worthwhile to use a goroutine and channels, but this solution would be quite appropriate for heavy computation.

Formulae
Θ = Angle of polar coordinates * π / 180.0 , where π=3.1416…

x of Cartesian = Radius of polar coordinates * Cos(Θ)

y of Cartesian = Radius of polar coordinates * Sin(Θ)

```go
package main
import (
  "bufio"
  "fmt"
  "math"
  "os"
  "runtime"
  "strconv"
  "strings"
)

type polar struct {
  radius, Θ  float64  // greek character!
}

type cartesian struct {
  x, y float64
}

const result = "Polar: radius=%.02f angle=%.02f degrees -- Cartesian: x=%.02f y=%.02f\n"

var prompt = "Enter a radius and an angle (in degrees), e.g., 12.5 90, " + "or %s to quit."

func init() {
  if runtime.GOOS == "windows" {
    prompt = fmt.Sprintf(prompt, "Ctrl+Z, Enter")
  } else { // Unix-like
    prompt = fmt.Sprintf(prompt, "Ctrl+C")
  }
}

func main() {
  questions := make(chan polar)
  defer close(questions)
  answers := createSolver(questions)
  defer close(answers)
  interact(questions, answers)
}

func createSolver(questions chan polar) chan cartesian {
  answers := make(chan cartesian)
  go func() {
    for {
      polarCoord := <-questions
      Θ := polarCoord.Θ * math.Pi / 180.0 // degrees to radians
      x := polarCoord.radius * math.Cos(Θ)
      y := polarCoord.radius * math.Sin(Θ)
      answers <- cartesian{x, y}
    }
  }()
  return answers
}

func interact(questions chan polar, answers chan cartesian) {
  reader := bufio.NewReader(os.Stdin)
  fmt.Println(prompt)
  for {
    fmt.Printf("Radius and angle: ")
    line, err := reader.ReadString('\n')
    if err != nil {
      break
    }
    line = line[:len(line)-1] // chop off newline character
    if numbers := strings.Fields(line); len(numbers) == 2 {
      polars, err := floatsToStrings(numbers)
      if err != nil {
        fmt.Fprintln(os.Stderr, "invalid number")
        continue
      }
      questions <- polar{polars[0], polars[1]}
      coord := <-answers
      fmt.Printf(result, polars[0], polars[1], coord.x, coord.y)
    } else {
      fmt.Fprintln(os.Stderr, "invalid input")
    }
  }
  fmt.Println()
}

func floatsToStrings(numbers []string) ([]float64, error) {
  var floats []float64
  for _, number := range numbers {
    if x, err := strconv.ParseFloat(number, 64); err != nil {
      return nil, err
    } else {
      floats = append(floats, x)
    }
  }
  return floats, nil
}
```
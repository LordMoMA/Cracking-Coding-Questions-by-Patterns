```go
package main
import "fmt"

type Any interface {}
type Anything struct {}

func main() {
    any := getAny()
    if any == nil {
        fmt.Println("any is nil")
    } else {
        fmt.Println("any is not nil")
    }
}

func getAny() Any {
    return getAnything()
}
func getAnything() *Anything {
    return nil
}
```

The output is "any is not nil" because getAnything() returns a nil pointer to Anything, and this nil pointer is then returned by getAny() as an Any interface.

In Go, an interface value is considered nil only if both its type and value are nil. In this case, the value is nil, but the type is *Anything, so the interface value is not nil.

If you want getAny() to return a nil interface value, you can do so by changing getAnything() to return an Any value:

```go
func getAnything() Any {
    return nil
}
```

Now getAnything() returns a nil interface value, and so does getAny(). As a result, the output of the program will be "any is nil".

In Go, an interface value is a combination of a type and a value. For example, if you have an interface i and you assign it a string "hello", the interface i now holds a value of "hello" and a type of string.

A nil interface value is one where both the type and the value are nil.

However, if you have a pointer type (like *Anything in your example), and you assign nil to that pointer, the interface now holds a value of nil and a type of *Anything. This is because the nil is the zero value for pointers, but the type of the pointer is still *Anything.

So even though the value is nil, the interface is not nil because it still holds a type. This is a common source of confusion in Go, but it's an important aspect of how interfaces work.

Here's a small example to illustrate this:

```go
type MyType struct{}

func main() {
    var i interface{}
    var p *MyType = nil

    i = p
    if i == nil {
        fmt.Println("i is nil")
    } else {
        fmt.Println("i is not nil")
    }
}
```

In this example, even though p is nil, when it's assigned to i, i is not nil because it holds a type (*MyType). So the output of this program is "i is not nil".

## A type classifier

In Go, a type classifier is not a built-in concept, but you can create functions to classify types using type assertions and type switches.

A type assertion provides access to an interface value's underlying concrete value.

```go
func classifier(items ...interface{}) {
  for i, x := range items {
    switch x.(type) {
    case bool: fmt.Printf("param #%d is a bool\n", i)
    case float64: fmt.Printf("param #%d is a float64\n", i)
    case int, int64: fmt.Printf("param #%d is an int\n", i)
    case nil: fmt.Printf("param #%d is nil\n", i)
    case string: fmt.Printf("param #%d is a string\n", i)

    default: fmt.Printf("param #%d's type is unknown\n", i)
    }
  }
}
```
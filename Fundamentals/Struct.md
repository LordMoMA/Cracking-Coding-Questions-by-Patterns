```go
package main
import (
  "fmt"
  "strings"
)

type Person struct {  // struct definition
  firstName string
  lastName string
}

func upPerson (p *Person) { // function using struct as a parameter
  p.firstName = strings.ToUpper(p.firstName)
  p.lastName = strings.ToUpper(p.lastName)
}

func main() {
  // 1- struct as a value type:
  var pers1 Person
  pers1.firstName = "Chris"
  pers1.lastName = "Woodward"
  upPerson(&pers1)
  fmt.Printf("The name of the person is %s %s\n", pers1.firstName, pers1.lastName)
  
  // 2 - struct as a pointer:
  pers2 := new(Person)
  pers2.firstName = "Chris"
  pers2.lastName = "Woodward"
  (*pers2).lastName = "Woodward" // this is also valid
  upPerson(pers2)
  fmt.Printf("The name of the person is %s %s\n", pers2.firstName, pers2.lastName)
  
  // 3 - struct as a literal:
  pers3 := &Person{"Chris","Woodward"}
  upPerson(pers3)
  fmt.Printf("The name of the person is %s %s\n", pers3.firstName, pers3.lastName)
}
```

## Struct Size

The size of T2 is not dependent on the size of T1, but rather on the size of the pointer it contains.

```c
typedef struct {
    int a[4]; // 4 integers, so size is 4 * sizeof(int)
} T1;

typedef struct {
    T1* t1_ptr; // Pointer to T1, so size is sizeof(T1*)
} T2;
```

In this case, sizeof(T1) would be 16 bytes (on a system where sizeof(int) is 4 bytes), because T1 contains an array of 4 integers. However, sizeof(T2) would be 8 bytes on a 64-bit system, because T2 contains a single pointer, and the size of a pointer on a 64-bit system is 8 bytes.

So, even though T1 is 16 bytes, T2 is only 8 bytes because it contains a pointer to T1, not an actual T1 structure. The size of a pointer is the same regardless of the type of data it points to.

```go
package main
import (
  "fmt"
  "unsafe"  // to use function Sizeof()
)

type T1 struct {  
  a, b int64
}

type T2 struct {
  t1 *T1  // pointer to T1
}

type T3 struct {
  t1 T1   // value type of T1
}

func main() {
  fmt.Println("Size of T1:", unsafe.Sizeof(T1{}))       // T1 value type
  fmt.Println("Size of T2:", unsafe.Sizeof(T2{&T1{}}))  // T2 containing pointer to T1
  fmt.Println("Size of T3:", unsafe.Sizeof(T3{}))       // Value of T3
}

// output
Size of T1: 16
Size of T2: 8
Size of T3: 16
```

## Conversion of structs

```go
type number struct {
	f	float32
}

type nr number   // new distinct type
type nrAlias = number // alias type

func main() {
    a := number{5.0}
    b := nr{5.0}
    c := nrAlias{5.0}
    // var i float32 = b   // compile-error: cannot use b (type nr) as type float32 in assignment
    // var i = float32(b)  // compile-error: cannot convert b (type nr) to type float32
    // var d number = b    // compile-error: cannot use b (type nr) as type number in assignment
    // needs a conversion:
    var d = number(b) // use explicit conversion to convert a nr to a number.
    // an alias doesn't need conversion:
    var e = b // var e nr = b also works
    fmt.Println(a, b, c, d, e)
}
```

nr is a new distinct type based on number. It means nr has the same underlying structure as number, but it's considered a completely different type by the Go compiler. You can't directly assign a value of type nr to a variable of type number or vice versa without explicit conversion.

## Tags: key “value” convention

Go allows the definition of multiple tags through the use of the key: "value" format. Look at the following example to see that once we have a tag, the value of its key can be retrieved through the Get method:

```go
package main
import (
"fmt"
"reflect"
)

type T struct {
  a int "This is a tag"
  b int `A raw string tag`
  c int `key1:"value1" key2:"value2"`
}

func main() {
  t := T{}
  fmt.Println(reflect.TypeOf(t).Field(0).Tag)
  if field, ok := reflect.TypeOf(t).FieldByName("b"); ok {
    fmt.Println(field.Tag)
  }
  if field, ok := reflect.TypeOf(t).FieldByName("c"); ok {
    fmt.Println(field.Tag)
    fmt.Println(field.Tag.Get("key1"))
  }
  if field, ok := reflect.TypeOf(t).FieldByName("d"); ok {
    fmt.Println(field.Tag)
  } else {
    fmt.Println("Field not found")
  }
}
```

## Anonymous Struct

```go
package main
import "fmt"

type C struct {   // struct 
    x float32      
    int      // int type anonymous field
    string     // string type anonymous field
  }

  func main() {
    c := C{3.14, 7, "hello"}  // making struct via literal expression
    fmt.Println(c.x, c.int, c.string) // output: 3.14 7 hello
    fmt.Println(c)  // output: {3.14 7 hello}
}
```

### Check what is wrong in below code:

```go
package main

import "container/list"

func (p *list.List) Iter() {
    // ...
}
func main() {
  lst := new(list.List)
  for _ = range lst.Iter() {
  }
}
```

in Go, you can't add methods to types from another package. This is a fundamental restriction in Go's type system to ensure that the behavior of a type is consistent and predictable no matter where it's used.

To work around this, you can define a new type in your own package that embeds the List type, and then add methods to your new type. Here's how you might do it:

```go
package main

import "container/list"

type MyList struct {
    *list.List
}

func (p *MyList) Iter() {
    // ...
}

func main() {
    lst := &MyList{list.New()}
    for _ = range lst.Iter() {
    }
}
```


### predict the output:

```go
package main

import "fmt"

type T struct {
    a int
    b float32
    c string
}

func main() {
    t := &T{ 7, -2.35, "abc\tdef" }
    fmt.Printf("%v\n", t)
}

func (t *T) String() string {
    return fmt.Sprintf("%d / %f / %q", t.a, t.b, t.c)
}

// 7 / -2.350000 / "abc\tdef"
```
In your main function, you're creating a new instance of T and printing it with fmt.Printf("%v\n", t). The %v verb is a default format and it prints your struct as-is. However, you've also defined a String method on T. In Go, if you define a method with this signature:

```go
func (t *T) String() string
```

Then whenever you print your type with fmt.Print or fmt.Printf, Go will automatically use this String method to convert your type to a string. This is because fmt.Print and fmt.Printf use the String method if it's available, as part of the fmt.Stringer interface.

So, when you do fmt.Printf("%v\n", t), Go is actually calling t.String() to get a string representation of t, and that's why you see 7 / -2.350000 / "abc\tdef" printed to the console.

The %f verb in the fmt.Sprintf function is used to format a floating-point number. By default, it formats the number with 6 digits after the decimal point. So, even though the number -2.35 only has two non-zero digits after the decimal point, fmt.Sprintf with %f will print it as -2.350000.

If you want to control the number of digits after the decimal point, you can do so by specifying a precision in the format specifier. For example, %.2f will print a floating-point number with exactly 2 digits after the decimal point. So fmt.Sprintf("%.2f", t.b) would print -2.35.


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

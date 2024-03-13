
```go
package main

import (
    "runtime"
    "time"
)

func main() {
    ms := runtime.MemStats{}
    runtime.ReadMemStats(&ms)

    println("Heap after GC. Used:", ms.HeapInuse, " Free:", ms.HeapIdle, " Meta:", ms.GCSys)

    time.Sleep(5 * time.Second)
}
```

```bash
➜  go run main.go
Heap after GC. Used: 393216  Free: 3571712  Meta: 2637696
```

HeapInuse: This is the number of bytes in in-use spans. In-use spans have at least one object which is being used, i.e., it's not ready for garbage collection. This is part of the total virtual memory Go's heap is using.

HeapIdle: This is the number of bytes in idle (unused) spans. Idle spans have no objects in them. These are essentially "free" and can be given back to the operating system, or used to service other memory requests.

GCSys: This is the number of bytes used for garbage collection system metadata. This includes the size of the metadata used by the garbage collector and other related structures.

So, the output "Heap after GC. Used: 393216 Free: 3571712 Meta: 2637696" means that after the last garbage collection, the heap is using 393216 bytes, there are 3571712 bytes in idle spans, and the garbage collection system is using 2637696 bytes for its metadata.

Suppose special action needs to be taken before an object obj is removed from memory, like writing to a log-file. This can be achieved by calling the function:

```go
runtime.SetFinalizer(obj, func(obj *typeObj))
```

The func(obj *typeObj) is a function that takes a pointer-parameter of the type of obj, which performs the additional action. func could also be an anonymous function.

SetFinalizer does not execute when the program comes to a normal end or when an error occurs, but before the object was chosen by the garbage collection process to be removed.

```go
package main

import (
    "fmt"
    "runtime"
    "time"
)

type File struct {
    name string
}

func (f *File) Close() {
    fmt.Println("Closing file:", f.name)
}

func main() {
    f := &File{name: "test.txt"}

    // Set a finalizer on f that will be called when f is garbage collected
    runtime.SetFinalizer(f, func(f *File) {
        f.Close()
    })

    // Set f to nil, making it eligible for garbage collection
    f = nil

    // Force a garbage collection cycle
    runtime.GC()

    // Wait to make sure the finalizer has run
    time.Sleep(1 * time.Second)
}
```
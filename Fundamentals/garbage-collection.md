
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
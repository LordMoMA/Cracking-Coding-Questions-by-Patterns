## Copying a file with a buffer

```go
package main

import (
    "fmt"
    "io"
    "os"
)

func main() {
    // Open original file
    originalFile, err := os.Open("original.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer originalFile.Close()

    // Create new file
    newFile, err := os.Create("new.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer newFile.Close()

    // Create a buffer to keep chunks that are read
    buffer := make([]byte, 100)

    for {
        // Read a chunk
        n, err := originalFile.Read(buffer) // bytes read is returned in n
        if err != nil && err != io.EOF {
            fmt.Println(err)
            return
        }
        if n == 0 {
            break
        }

        // Write a chunk
        if _, err := newFile.Write(buffer[:n]); err != nil {
            fmt.Println(err)
            return
        }
    }
}
```

Under the hood, the Read and Write functions are system calls that interact with the operating system's file system. When you call Read, the operating system reads data from the file into the program's memory. When you call Write, the operating system writes data from the program's memory to the file. The buffer is a slice of bytes in the program's memory that is used to hold the data being read or written.
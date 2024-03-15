
## Recovering to stop a panic terminating sequence:

In Go, a panic is a runtime error that can be triggered by the panic function. When a panic occurs, execution stops, deferred functions (if any) are executed, and then the program crashes.

However, you can "recover" from a panic and prevent the program from crashing by using the recover function. The recover function stops the panic and returns the value that was passed to panic. If recover is called outside of a deferred function or if no panic is happening, it does nothing and returns nil.

```go
package main

import (
    "errors"
    "log"
)

func protect(g func()) {
    defer func() {
        log.Println("done") // Println executes normally even if there is a panic
        if x := recover(); x != nil {
            log.Printf("run time panic: %v", x)
        }
    }()
    log.Println("start")
    g() // Execute the function
}

func main() {
    protect(func() {
        log.Println("Inside function before panic")
        panic(errors.New("an error occurred"))     // This will cause a panic
        log.Println("Inside function after panic") // This won't be executed
    })

    executeFurther()
}

// output:
2024/03/15 15:30:12 start
2024/03/15 15:30:12 Inside function before panic
2024/03/15 15:30:12 done
2024/03/15 15:30:12 run time panic: an error occurred
```

the executeFurther() function will still be executed even if a panic occurs in the protect function. This is because the protect function uses recover to stop the panic from crashing the program.

When a panic is called, like panic(errors.New("an error occurred")) in your selected code, the current function stops executing immediately.

Any functions that were deferred (scheduled to run at the end of the function) will still run, even if a panic happens.

If a recover is called inside one of these deferred functions, it stops the panic. This means the program won't crash, and it can continue running as if nothing happened.

However, any code after the panic in the same function won't run. So in your example, log.Println("Inside function after panic") won't be executed if the panic happens before it.

Now, when to use panic vs normal error handling:

Use panic for serious programming errors that should stop the entire program, like dereferencing a nil pointer or an out-of-bounds slice access. These are issues that typically indicate a bug in the code.

Use normal error handling for errors that are expected and can be recovered from, like failing to open a file that might not exist, or failing to connect to a database that might be temporarily down. These are issues that can often be resolved by retrying the operation, or by informing the user and letting them decide what to do.

### To sum up:

If a panic occurs anywhere in the main function, the deferred function will be executed before the program crashes. This function calls recover, which stops the panic and returns the value that was passed to panic. The program then continues executing as normal.

In your code, the protect function uses recover in a similar way to catch and handle any panic that occurs in the function g. If g causes a panic, the deferred function in protect will be executed, recover will stop the panic, and the value passed to panic will be logged.

In Go, panic is typically used for errors that should not be handled or cannot be recovered from, and should result in the program terminating. It's often used for programming errors, such as dereferencing a nil pointer or failing to handle a returned error.

In your code, panic is used to simulate an error that cannot be recovered from. It's used in a function that's passed to protect, which is designed to recover from panics. This is a way to demonstrate how protect works.

In real-world code, you would typically use error handling for recoverable errors, and reserve panic for unrecoverable errors. If a function can return an error, it should return an error rather than panicking.

However, there are some cases where you might want to use panic in a function that's passed to another function like protect. For example, if you're writing a web server, you might want to recover from panics in your request handlers to prevent a single request from bringing down your entire server. In this case, protect could be a middleware that recovers from panics in your handlers.


[code example from Caddy to use recover()](https://github.com/caddyserver/caddy/blob/52822a41cb94fcc9669cd7ba8ac1a0513e4d55f9/modules/caddyhttp/reverseproxy/metrics.go#L50)

```go
func (m *metricsUpstreamsHealthyUpdater) Init() {
    go func() {
        defer func() {
            if err := recover(); err != nil {
                reverseProxyMetrics.logger.Error("upstreams healthy metrics updater panicked",
                    zap.Any("error", err),
                    zap.ByteString("stack", debug.Stack()))
            }
        }()

        m.update()

        ticker := time.NewTicker(10 * time.Second)
        for {
            select {
            case <-ticker.C:
                m.update()
            case <-m.handler.ctx.Done():
                ticker.Stop()
                return
            }
        }
    }()
}
```

This is a good example of using recover in Go.

The Init function starts a goroutine that periodically updates some metrics. If an unhandled error or panic occurs in the update method or anywhere else in the goroutine, the deferred function will recover from the panic, log an error message with the panic details and the stack trace, and then allow the goroutine to exit gracefully.

This is a good practice because it prevents a panic in one goroutine from bringing down the entire program. It also logs useful information about the panic, which can help with debugging.

However, it's important to note that recovering from a panic is not a substitute for proper error handling. You should still try to handle errors gracefully in your code and only use recover as a last resort to prevent the program from crashing.

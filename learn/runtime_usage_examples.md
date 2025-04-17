# Go runtime Package: Common Functions and Examples

The `runtime` package provides operations that interact with Go's runtime system.

## Main Functions and Types
- `runtime.Gosched()`: Yields the processor, allowing other goroutines to run.
- `runtime.GOMAXPROCS(n int) int`: Sets the maximum number of CPUs that can execute simultaneously.
- `runtime.NumCPU() int`: Returns the number of logical CPUs.
- `runtime.Goexit()`: Terminates the current goroutine.
- `runtime.Caller(skip int) (pc uintptr, file string, line int, ok bool)`: Reports file and line number information.
- `runtime.Stack(buf []byte, all bool) int`: Writes stack trace to buf.
- `runtime.ReadMemStats(m *runtime.MemStats)`: Populates m with memory allocator statistics.
- `type runtime.MemStats`: Memory statistics structure.

## Example: CPU and Goroutine Management
```go
package main
import (
    "runtime"
    "fmt"
    "time"
)

func main() {
    // Get CPU information
    fmt.Printf("Number of CPUs: %d\n", runtime.NumCPU())
    
    // Set maximum number of CPUs
    oldMaxProcs := runtime.GOMAXPROCS(2)
    fmt.Printf("Previous GOMAXPROCS: %d\n", oldMaxProcs)
    
    // Demonstrate Gosched
    go func() {
        for i := 0; i < 3; i++ {
            fmt.Printf("Background: %d\n", i)
            runtime.Gosched() // Yield to other goroutines
        }
    }()
    
    for i := 0; i < 3; i++ {
        fmt.Printf("Foreground: %d\n", i)
        time.Sleep(1 * time.Millisecond)
    }
}
```

## Example: Stack Traces and Caller Information
```go
package main
import (
    "runtime"
    "fmt"
)

func traceCaller() {
    // Get caller information
    pc, file, line, ok := runtime.Caller(0)
    if ok {
        fn := runtime.FuncForPC(pc)
        fmt.Printf("Function: %s\n", fn.Name())
        fmt.Printf("File: %s:%d\n", file, line)
    }
    
    // Get full stack trace
    buf := make([]byte, 1024)
    n := runtime.Stack(buf, false)
    fmt.Printf("Stack trace:\n%s\n", buf[:n])
}

func main() {
    traceCaller()
}
```

## Example: Memory Statistics
```go
package main
import (
    "runtime"
    "fmt"
)

func main() {
    var m runtime.MemStats
    runtime.ReadMemStats(&m)
    
    fmt.Println("Memory Statistics:")
    fmt.Printf("Alloc = %v MiB\n", m.Alloc/1024/1024)
    fmt.Printf("TotalAlloc = %v MiB\n", m.TotalAlloc/1024/1024)
    fmt.Printf("Sys = %v MiB\n", m.Sys/1024/1024)
    fmt.Printf("NumGC = %v\n", m.NumGC)
    fmt.Printf("GCCPUFraction = %v%%\n", m.GCCPUFraction*100)
}
```

## Example: Goroutine Exit and Cleanup
```go
package main
import (
    "runtime"
    "fmt"
    "sync"
)

func cleanup() {
    if r := recover(); r != nil {
        fmt.Println("Recovered from:", r)
    }
}

func worker(wg *sync.WaitGroup) {
    defer wg.Done()
    defer cleanup()
    
    fmt.Println("Worker starting...")
    
    go func() {
        fmt.Println("Nested goroutine running...")
        runtime.Goexit() // Terminate just this goroutine
        // This line will never be reached
    }()
    
    // Wait a moment to show the effect
    runtime.Gosched()
    fmt.Println("Worker finishing...")
}

func main() {
    var wg sync.WaitGroup
    wg.Add(1)
    
    go worker(&wg)
    wg.Wait()
    
    fmt.Printf("Final goroutine count: %d\n", runtime.NumGoroutine())
}
```

## Example: Runtime Version and Build Info
```go
package main
import (
    "runtime"
    "fmt"
)

func main() {
    fmt.Printf("Go Version: %s\n", runtime.Version())
    fmt.Printf("Go Arch: %s\n", runtime.GOARCH)
    fmt.Printf("Go OS: %s\n", runtime.GOOS)
    
    // Get garbage collection info
    fmt.Printf("NumCPU: %d\n", runtime.NumCPU())
    fmt.Printf("NumGoroutine: %d\n", runtime.NumGoroutine())
    fmt.Printf("NumCgoCall: %d\n", runtime.NumCgoCall())
}
```

See the [official documentation](https://pkg.go.dev/runtime) for more details.

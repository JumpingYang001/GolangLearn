# Go runtime/pprof Package: Common Functions and Examples

The `runtime/pprof` package provides functions for CPU and memory profiling.

## Main Functions and Types
- `pprof.StartCPUProfile(w io.Writer) error`: Starts CPU profiling.
- `pprof.StopCPUProfile()`: Stops the current CPU profile.
- `pprof.WriteHeapProfile(w io.Writer) error`: Writes a heap profile.
- `pprof.Lookup(name string) *pprof.Profile`: Looks up a profile by name.
- `type pprof.Profile`: Represents a profile.

## Example: CPU Profiling
```go
package main
import (
    "os"
    "runtime/pprof"
    "log"
)

func cpuIntensiveTask() {
    // Simulate CPU-intensive work
    for i := 0; i < 1000; i++ {
        for j := 0; j < 1000; j++ {
            _ = i * j
        }
    }
}

func main() {
    // Create CPU profile file
    f, err := os.Create("cpu.prof")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    // Start CPU profiling
    if err := pprof.StartCPUProfile(f); err != nil {
        log.Fatal(err)
    }
    defer pprof.StopCPUProfile()

    // Run the task we want to profile
    cpuIntensiveTask()
}
```

## Example: Memory (Heap) Profiling
```go
package main
import (
    "os"
    "runtime/pprof"
    "log"
)

func allocateMemory() [][]int {
    var data [][]int
    for i := 0; i < 100; i++ {
        // Allocate some memory
        x := make([]int, 1000)
        data = append(data, x)
    }
    return data
}

func main() {
    // Allocate memory
    data := allocateMemory()
    _ = data // Keep the reference

    // Create heap profile
    f, err := os.Create("heap.prof")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()

    // Write heap profile
    if err := pprof.WriteHeapProfile(f); err != nil {
        log.Fatal(err)
    }
}
```

## Example: Working with Profiles
```go
package main
import (
    "os"
    "runtime/pprof"
    "log"
    "fmt"
)

func main() {
    // List all available profiles
    profiles := pprof.Profiles()
    fmt.Println("Available profiles:")
    for _, p := range profiles {
        fmt.Printf("- %s: %d samples\n", p.Name(), p.Count())
    }

    // Look up specific profiles
    heap := pprof.Lookup("heap")
    if heap != nil {
        f, err := os.Create("current_heap.prof")
        if err != nil {
            log.Fatal(err)
        }
        defer f.Close()
        heap.WriteTo(f, 0)
    }

    goroutine := pprof.Lookup("goroutine")
    if goroutine != nil {
        f, err := os.Create("goroutines.prof")
        if err != nil {
            log.Fatal(err)
        }
        defer f.Close()
        goroutine.WriteTo(f, 0)
    }
}
```

## Example: HTTP Server with Profiling
```go
package main
import (
    "net/http"
    _ "net/http/pprof" // Automatically registers profiling endpoints
    "log"
)

func main() {
    // Start HTTP server with profiling endpoints
    // Access profiles at:
    // - /debug/pprof/
    // - /debug/pprof/heap
    // - /debug/pprof/goroutine
    // - /debug/pprof/profile (30-second CPU profile)
    // - /debug/pprof/trace
    log.Println("Starting server on :6060")
    log.Fatal(http.ListenAndServe(":6060", nil))
}
```

## Usage Notes
To analyze profiles:
1. Generate profile: `go run main.go`
2. Use pprof tool:
   ```
   go tool pprof cpu.prof
   go tool pprof http://localhost:6060/debug/pprof/heap
   ```
3. Common pprof commands:
   - `top`: Show top consumers
   - `web`: Visualize graph (requires graphviz)
   - `list funcName`: Show source line hot spots

See the [official documentation](https://pkg.go.dev/runtime/pprof) for more details.

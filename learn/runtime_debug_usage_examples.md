# Go runtime/debug Package: Common Functions and Examples

The `runtime/debug` package provides facilities for debugging Go programs.

## Main Functions and Types
- `debug.PrintStack()`: Prints the stack trace to standard error.
- `debug.ReadBuildInfo() (*debug.BuildInfo, bool)`: Returns build information.
- `debug.SetGCPercent(percent int) int`: Sets the garbage collection target percentage.
- `debug.FreeOSMemory()`: Forces a garbage collection and returns memory to the OS.
- `debug.SetMaxStack(bytes int) int`: Sets the maximum stack size.
- `debug.SetPanicOnFault(enabled bool)`: Controls whether the program should panic on a fault.
- `type debug.BuildInfo`: Contains build information.

## Example: Stack Traces and Build Info
```go
package main
import (
    "runtime/debug"
    "fmt"
)

func someFunction() {
    // Print current stack trace
    fmt.Println("Stack trace:")
    debug.PrintStack()
}

func main() {
    // Print stack trace
    someFunction()
    
    // Get build information
    if info, ok := debug.ReadBuildInfo(); ok {
        fmt.Printf("Main package path: %s\n", info.Path)
        fmt.Printf("Main version: %s\n", info.Main.Version)
        fmt.Println("Dependencies:")
        for _, dep := range info.Deps {
            fmt.Printf("- %s@%s\n", dep.Path, dep.Version)
        }
    }
}
```

## Example: Memory Management
```go
package main
import (
    "runtime/debug"
    "fmt"
    "runtime"
)

func main() {
    // Set garbage collection target
    oldPercent := debug.SetGCPercent(100)
    fmt.Printf("Old GC percent: %d\n", oldPercent)
    
    // Force garbage collection and memory return
    fmt.Println("Memory stats before:")
    printMemStats()
    
    debug.FreeOSMemory()
    
    fmt.Println("\nMemory stats after:")
    printMemStats()
}

func printMemStats() {
    var mem runtime.MemStats
    runtime.ReadMemStats(&mem)
    fmt.Printf("Allocated: %d MB\n", mem.Alloc/1024/1024)
    fmt.Printf("System Memory: %d MB\n", mem.Sys/1024/1024)
}
```

## Example: Runtime Settings
```go
package main
import (
    "runtime/debug"
    "fmt"
)

func main() {
    // Set maximum stack size (8MB)
    oldMaxStack := debug.SetMaxStack(8 * 1024 * 1024)
    fmt.Printf("Old max stack: %d bytes\n", oldMaxStack)
    
    // Enable panic on memory fault
    debug.SetPanicOnFault(true)
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
        }
    }()
    
    // Example of using build info in error reporting
    if info, ok := debug.ReadBuildInfo(); ok {
        fmt.Println("Error reporting info:")
        fmt.Printf("Go version: %s\n", info.GoVersion)
        fmt.Printf("Module path: %s\n", info.Path)
        settings := map[string]string{}
        for _, s := range info.Settings {
            settings[s.Key] = s.Value
        }
        if vcs, ok := settings["vcs"]; ok {
            fmt.Printf("Version control: %s\n", vcs)
        }
        if rev, ok := settings["vcs.revision"]; ok {
            fmt.Printf("Revision: %s\n", rev)
        }
    }
}
```

See the [official documentation](https://pkg.go.dev/runtime/debug) for more details.

# Go os Package: Common Functions and Examples

The `os` package provides a platform-independent interface to operating system functionality.

## Main Functions and Types
- `os.Open(name string) (*os.File, error)`: Opens a file for reading.
- `os.Create(name string) (*os.File, error)`: Creates a file for writing.
- `os.Remove(name string) error`: Removes a file or directory.
- `os.Mkdir(name string, perm os.FileMode) error`: Creates a directory.
- `os.Stat(name string) (os.FileInfo, error)`: Gets file or directory info.
- `os.Getenv(key string) string`: Gets an environment variable.
- `os.Setenv(key, value string) error`: Sets an environment variable.
- `os.Exit(code int)`: Exits the program.
- `type os.File`, `type os.FileInfo`: File and file info types.

## Example: File Operations
```go
package main
import (
    "os"
    "log"
    "fmt"
)

func main() {
    // Create and write to a file
    file, err := os.Create("example.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    file.WriteString("Hello, file!")

    // Open and read from a file
    file2, err := os.Open("example.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file2.Close()
    buf := make([]byte, 100)
    n, err := file2.Read(buf)
    fmt.Println(string(buf[:n]))

    // Get file info using Stat
    info, err := os.Stat("example.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Name: %s, Size: %d, Mode: %v, ModTime: %v\n",
        info.Name(), info.Size(), info.Mode(), info.ModTime())

    // Remove the file
    err = os.Remove("example.txt")
    if err != nil {
        log.Fatal(err)
    }
}
```

## Example: Directory Operations
```go
package main
import (
    "os"
    "log"
    "fmt"
)

func main() {
    // Create a directory
    err := os.Mkdir("mydir", 0755)
    if err != nil {
        log.Fatal(err)
    }

    // List files in directory
    entries, err := os.ReadDir(".")
    if err != nil {
        log.Fatal(err)
    }
    for _, entry := range entries {
        info, err := entry.Info()
        if err != nil {
            continue
        }
        fmt.Printf("Name: %s, IsDir: %v\n", entry.Name(), entry.IsDir())
    }

    // Clean up
    os.Remove("mydir")
}
```

## Example: Environment Variables and Program Exit
```go
package main
import (
    "os"
    "fmt"
)

func main() {
    // Set environment variable
    err := os.Setenv("MY_VAR", "my_value")
    if err != nil {
        fmt.Println("Error setting env var:", err)
        os.Exit(1)
    }

    // Get environment variable
    value := os.Getenv("MY_VAR")
    fmt.Println("MY_VAR =", value)

    // Example of using Exit
    if value == "" {
        fmt.Println("MY_VAR not set")
        os.Exit(1)
    }
    fmt.Println("Program completed successfully")
    os.Exit(0)
}
```

See the [official documentation](https://pkg.go.dev/os) for more details.

# Go path/filepath Package: Common Functions and Examples

The `path/filepath` package provides utilities for manipulating filename paths in a way compatible with the target operating system.

## Main Functions and Types
- `filepath.Base(path string) string`: Returns the last element of the path.
- `filepath.Dir(path string) string`: Returns all but the last element.
- `filepath.Ext(path string) string`: Returns the file extension.
- `filepath.Join(elem ...string) string`: Joins any number of path elements.
- `filepath.Clean(path string) string`: Cleans up a path.
- `filepath.Split(path string) (dir, file string)`: Splits a path into directory and file.
- `filepath.Abs(path string) (string, error)`: Returns an absolute representation of path.
- `filepath.Glob(pattern string) ([]string, error)`: Returns the names of all files matching pattern.
- `filepath.WalkDir(root string, fn fs.WalkDirFunc) error`: Walks the file tree rooted at root.

## Example: Basic Path Operations
```go
package main
import (
    "path/filepath"
    "fmt"
)

func main() {
    path := "/home/user/docs/file.txt"
    
    // Base and Dir
    fmt.Println("Base:", filepath.Base(path))  // file.txt
    fmt.Println("Dir:", filepath.Dir(path))    // /home/user/docs
    
    // Extension
    fmt.Println("Extension:", filepath.Ext(path))  // .txt
    
    // Join paths
    newPath := filepath.Join("a", "b", "c.txt")
    fmt.Println("Joined path:", newPath)
    
    // Clean path
    messyPath := "a//b/../c/./d"
    fmt.Println("Cleaned path:", filepath.Clean(messyPath))
}
```

## Example: Path Analysis
```go
package main
import (
    "path/filepath"
    "fmt"
)

func main() {
    path := "/home/user/docs/file.txt"
    
    // Split into directory and file
    dir, file := filepath.Split(path)
    fmt.Printf("Directory: %s, File: %s\n", dir, file)
    
    // Get absolute path
    abs, err := filepath.Abs("relative/path")
    if err != nil {
        panic(err)
    }
    fmt.Println("Absolute path:", abs)
}
```

## Example: File Pattern Matching and Walking
```go
package main
import (
    "path/filepath"
    "fmt"
    "io/fs"
)

func main() {
    // Find all .txt files
    matches, err := filepath.Glob("*.txt")
    if err != nil {
        panic(err)
    }
    fmt.Println("Text files:", matches)
    
    // Walk directory tree
    err = filepath.WalkDir(".", func(path string, d fs.DirEntry, err error) error {
        if err != nil {
            return err
        }
        if !d.IsDir() {
            fmt.Printf("File: %s\n", path)
        }
        return nil
    })
    if err != nil {
        panic(err)
    }
}
```

## Example: Working with Relative Paths
```go
package main
import (
    "path/filepath"
    "fmt"
)

func main() {
    // Get relative path
    rel, err := filepath.Rel("/home/user", "/home/user/docs/file.txt")
    if err != nil {
        panic(err)
    }
    fmt.Println("Relative path:", rel)
    
    // Evaluate symbolic links
    eval, err := filepath.EvalSymlinks("link.txt")
    if err != nil {
        fmt.Println("EvalSymlinks error:", err)
    } else {
        fmt.Println("Evaluated path:", eval)
    }
}
```

See the [official documentation](https://pkg.go.dev/path/filepath) for more details.

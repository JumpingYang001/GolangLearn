# Go path Package: Common Functions and Examples

The `path` package provides utilities for manipulating slash-separated paths.

## Main Functions and Types
- `path.Base(path string) string`: Returns the last element of the path.
- `path.Dir(path string) string`: Returns all but the last element.
- `path.Ext(path string) string`: Returns the file extension.
- `path.Join(elem ...string) string`: Joins any number of path elements.
- `path.Clean(path string) string`: Cleans up a path.
- `path.Split(path string) (dir, file string)`: Splits a path into directory and file.

## Example: Basic Path Operations
```go
package main
import (
    "path"
    "fmt"
)

func main() {
    // Note: path package always uses forward slashes
    p := "/home/user/docs/file.txt"
    
    // Base and Dir
    fmt.Println("Base:", path.Base(p))     // file.txt
    fmt.Println("Dir:", path.Dir(p))       // /home/user/docs
    
    // Extension
    fmt.Println("Extension:", path.Ext(p)) // .txt
    
    // Join paths (always uses forward slashes)
    newPath := path.Join("a", "b", "c.txt")
    fmt.Println("Joined:", newPath)        // a/b/c.txt
}
```

## Example: Path Cleaning and Splitting
```go
package main
import (
    "path"
    "fmt"
)

func main() {
    // Clean messy paths
    messy := "a//b/../c/./d"
    clean := path.Clean(messy)
    fmt.Println("Cleaned path:", clean)    // a/c/d
    
    // Split path into directory and file
    dir, file := path.Split("/home/user/file.txt")
    fmt.Printf("Directory: %s, File: %s\n", dir, file)
    // Directory: /home/user/, File: file.txt
    
    // Important: Use path package for URL paths
    urlPath := "/blog///post/../..//index.html"
    fmt.Println("Clean URL path:", path.Clean(urlPath)) // /index.html
}
```

## Example: Working with URL Paths
```go
package main
import (
    "path"
    "fmt"
)

func main() {
    // path package is useful for URL paths
    urlPath := "/api/v1//users/"
    fmt.Println("Base:", path.Base(urlPath))        // users
    fmt.Println("Dir:", path.Dir(urlPath))          // /api/v1
    fmt.Println("Clean:", path.Clean(urlPath))      // /api/v1/users
    
    // Join URL path components
    apiPath := path.Join("api", "v2", "posts")
    fmt.Println("API path:", apiPath)               // api/v2/posts
}
```

See the [official documentation](https://pkg.go.dev/path) for more details.

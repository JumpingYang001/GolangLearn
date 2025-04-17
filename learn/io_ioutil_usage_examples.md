# Go io/ioutil Package: Common Functions and Examples

The `io/ioutil` package provides I/O utility functions for working with files and data. (Note: Deprecated since Go 1.16, use `os` and `io` instead.)

## Main Functions and Types
- `ioutil.ReadFile(filename string) ([]byte, error)`: Reads a file into memory.
- `ioutil.WriteFile(filename string, data []byte, perm os.FileMode) error`: Writes data to a file.
- `ioutil.ReadAll(r io.Reader) ([]byte, error)`: Reads all data from a reader.
- `ioutil.TempFile(dir, pattern string) (*os.File, error)`: Creates a temporary file.
- `ioutil.TempDir(dir, pattern string) (string, error)`: Creates a temporary directory.

## Example: Reading a File
```go
package main
import (
    "io/ioutil"
    "fmt"
    "log"
)

func main() {
    data, err := ioutil.ReadFile("input.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(data))
}
```

## Example: Writing a File
```go
package main
import (
    "io/ioutil"
    "log"
)

func main() {
    err := ioutil.WriteFile("output.txt", []byte("Hello, ioutil!"), 0644)
    if err != nil {
        log.Fatal(err)
    }
}
```

## Example: ReadAll
```go
package main
import (
    "io/ioutil"
    "strings"
    "fmt"
)

func main() {
    r := strings.NewReader("Read all this text!")
    data, _ := ioutil.ReadAll(r)
    fmt.Println(string(data))
}
```

## Example: TempFile
```go
package main
import (
    "io/ioutil"
    "fmt"
    "os"
)

func main() {
    f, err := ioutil.TempFile("", "example")
    if err != nil {
        panic(err)
    }
    defer os.Remove(f.Name())
    fmt.Println("Temp file created:", f.Name())
}
```

## Example: TempDir
```go
package main
import (
    "io/ioutil"
    "fmt"
    "os"
)

func main() {
    dir, err := ioutil.TempDir("", "exampledir")
    if err != nil {
        panic(err)
    }
    defer os.RemoveAll(dir)
    fmt.Println("Temp dir created:", dir)
}
```

See the [official documentation](https://pkg.go.dev/io/ioutil) for more details.

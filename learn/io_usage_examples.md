# Go io Package: Common Functions and Examples

The `io` package provides basic interfaces to I/O primitives.

## Main Functions and Types
- `io.Reader`, `io.Writer`: Core interfaces for reading and writing.
- `io.Copy(dst Writer, src Reader) (written int64, err error)`: Copies data from src to dst.
- `io.ReadFull(r Reader, buf []byte) (n int, err error)`: Reads exactly len(buf) bytes.
- `io.TeeReader(r Reader, w Writer) Reader`: Returns a Reader that writes to w what it reads from r.
- `io.Pipe() (*PipeReader, *PipeWriter)`: Synchronous in-memory pipe.
- `io.EOF`, `io.ErrUnexpectedEOF`: Common error values.

## Example: Using io.Reader and io.Writer
```go
package main
import (
    "io"
    "os"
    "log"
)

func main() {
    src, err := os.Open("input.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer src.Close()

    dst, err := os.Create("output.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer dst.Close()

    io.Copy(dst, src)
}
```

## Example: Using io.ReadFull
```go
package main
import (
    "io"
    "os"
    "fmt"
)

func main() {
    f, _ := os.Open("input.txt")
    defer f.Close()
    buf := make([]byte, 5)
    n, err := io.ReadFull(f, buf)
    fmt.Println(n, err, string(buf))
}
```

## Example: Using io.TeeReader
```go
package main
import (
    "io"
    "os"
)

func main() {
    src, _ := os.Open("input.txt")
    defer src.Close()
    tee := io.TeeReader(src, os.Stdout)
    io.ReadAll(tee)
}
```

## Example: Using io.Pipe
```go
package main
import (
    "io"
    "fmt"
)

func main() {
    r, w := io.Pipe()
    go func() {
        w.Write([]byte("hello"))
        w.Close()
    }()
    buf := make([]byte, 5)
    r.Read(buf)
    fmt.Println(string(buf))
}
```

## Example: Using io.EOF and io.ErrUnexpectedEOF
```go
package main
import (
    "io"
    "strings"
    "fmt"
)

func main() {
    r := strings.NewReader("hi")
    buf := make([]byte, 5)
    n, err := r.Read(buf)
    fmt.Println(n, err == io.EOF, err == io.ErrUnexpectedEOF)
}
```

See the [official documentation](https://pkg.go.dev/io) for more details.

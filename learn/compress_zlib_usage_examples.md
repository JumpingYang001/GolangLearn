# Go compress/zlib Package: Common Functions and Examples

The `compress/zlib` package provides support for reading and writing files in zlib format.

## Main Functions and Types
- `zlib.NewReader(r io.Reader) (*zlib.Reader, error)`: Creates a new zlib reader.
- `zlib.NewWriter(w io.Writer) *zlib.Writer`: Creates a new zlib writer.
- `(*zlib.Reader).Read(b []byte) (int, error)`: Reads uncompressed data.
- `(*zlib.Writer).Write(b []byte) (int, error)`: Writes compressed data.
- `(*zlib.Writer).Close() error`: Closes the writer, flushing any unwritten data.
- `(*zlib.Reader).Close() error`: Closes the reader.

## Example: Writing zlib data
```go
package main
import (
    "compress/zlib"
    "os"
    "log"
)

func main() {
    file, err := os.Create("test.txt.zlib")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    zw := zlib.NewWriter(file)
    defer zw.Close()

    zw.Write([]byte("Hello, zlib!"))
}
```

## Example: Reading zlib data
```go
package main
import (
    "compress/zlib"
    "os"
    "io"
    "log"
    "fmt"
)

func main() {
    file, err := os.Open("test.txt.zlib")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    zr, err := zlib.NewReader(file)
    if err != nil {
        log.Fatal(err)
    }
    defer zr.Close()

    data, err := io.ReadAll(zr)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(data))
}
```

See the [official documentation](https://pkg.go.dev/compress/zlib) for more details.

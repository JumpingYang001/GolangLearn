# Go compress/gzip Package: Common Functions and Examples

The `compress/gzip` package provides support for reading and writing files in gzip format.

## Main Functions and Types
- `gzip.NewReader(r io.Reader) (*gzip.Reader, error)`: Creates a new gzip reader.
- `gzip.NewWriter(w io.Writer) *gzip.Writer`: Creates a new gzip writer.
- `(*gzip.Reader).Read(b []byte) (int, error)`: Reads uncompressed data.
- `(*gzip.Writer).Write(b []byte) (int, error)`: Writes compressed data.
- `(*gzip.Writer).Close() error`: Closes the writer, flushing any unwritten data.
- `(*gzip.Reader).Close() error`: Closes the reader.
- `type gzip.Header`: Metadata for gzip files.

## Example: Writing gzip data
```go
package main
import (
    "compress/gzip"
    "os"
    "log"
)

func main() {
    file, err := os.Create("test.txt.gz")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    gw := gzip.NewWriter(file)
    defer gw.Close()

    gw.Write([]byte("Hello, gzip!"))
}
```

## Example: Reading gzip data
```go
package main
import (
    "compress/gzip"
    "os"
    "io"
    "log"
    "fmt"
)

func main() {
    file, err := os.Open("test.txt.gz")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    gr, err := gzip.NewReader(file)
    if err != nil {
        log.Fatal(err)
    }
    defer gr.Close()

    data, err := io.ReadAll(gr)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(data))
}
```

See the [official documentation](https://pkg.go.dev/compress/gzip) for more details.

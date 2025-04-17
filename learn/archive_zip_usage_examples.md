# Go archive/zip Package: Common Functions and Examples

The `archive/zip` package provides support for reading and writing ZIP archives.

## Main Functions and Types
- `zip.NewReader(r io.ReaderAt, size int64) (*zip.Reader, error)`: Creates a new ZIP archive reader.
- `zip.NewWriter(w io.Writer) *zip.Writer`: Creates a new ZIP archive writer.
- `zip.OpenReader(name string) (*zip.ReadCloser, error)`: Opens a ZIP file for reading.
- `(*zip.Writer).Create(name string) (io.Writer, error)`: Adds a new file to the ZIP archive.
- `(*zip.Writer).CreateHeader(fh *zip.FileHeader) (io.Writer, error)`: Adds a file with a custom header.
- `(*zip.Reader).File`: Slice of files in the archive.
- `type zip.File`: Represents a file in the archive.
- `type zip.FileHeader`: Metadata for files in the archive.

## Example: Creating a ZIP file
```go
package main
import (
    "archive/zip"
    "os"
    "log"
)

func main() {
    file, err := os.Create("test.zip")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    zw := zip.NewWriter(file)
    defer zw.Close()

    w, err := zw.Create("hello.txt")
    if err != nil {
        log.Fatal(err)
    }
    _, err = w.Write([]byte("Hello, zip!"))
    if err != nil {
        log.Fatal(err)
    }
}
```

## Example: Creating a ZIP file with a custom header
```go
package main
import (
    "archive/zip"
    "os"
    "log"
    "time"
)

func main() {
    file, err := os.Create("test_custom.zip")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    zw := zip.NewWriter(file)
    defer zw.Close()

    header := &zip.FileHeader{
        Name:     "custom.txt",
        Method:   zip.Deflate,
        Modified: time.Now(),
    }
    w, err := zw.CreateHeader(header)
    if err != nil {
        log.Fatal(err)
    }
    _, err = w.Write([]byte("Hello, custom header!"))
    if err != nil {
        log.Fatal(err)
    }
}
```

## Example: Reading a ZIP file
```go
package main
import (
    "archive/zip"
    "log"
    "fmt"
)

func main() {
    r, err := zip.OpenReader("test.zip")
    if err != nil {
        log.Fatal(err)
    }
    defer r.Close()

    for _, f := range r.File {
        fmt.Println("File in zip:", f.Name)
    }
}
```

## Example: Reading a ZIP file from an io.ReaderAt (zip.NewReader)
```go
package main
import (
    "archive/zip"
    "os"
    "log"
    "fmt"
)

func main() {
    file, err := os.Open("test.zip")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    fi, err := file.Stat()
    if err != nil {
        log.Fatal(err)
    }

    zr, err := zip.NewReader(file, fi.Size())
    if err != nil {
        log.Fatal(err)
    }

    for _, f := range zr.File {
        fmt.Println("File in zip (NewReader):", f.Name)
    }
}
```

See the [official documentation](https://pkg.go.dev/archive/zip) for more details.

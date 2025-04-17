# Go mime/multipart Package: Common Functions and Examples

The `mime/multipart` package provides support for multipart MIME data, commonly used for file uploads in HTTP.

## Main Functions and Types
- `multipart.NewReader(r io.Reader, boundary string) *multipart.Reader`: Parses multipart data from a reader.
- `multipart.NewWriter(w io.Writer) *multipart.Writer`: Creates a new multipart writer.
- `(*multipart.Writer).CreateFormFile(fieldname, filename string) (io.Writer, error)`: Creates a new form-data header for a file.
- `(*multipart.Writer).WriteField(fieldname, value string) error`: Adds a simple field.
- `(*multipart.Reader).NextPart() (*multipart.Part, error)`: Advances to the next part.
- `type multipart.Part`: Represents a single part in the multipart data.
- `type multipart.File`: Interface for uploaded files.

## Example: Creating Multipart Form Data (Writer)
```go
package main
import (
    "bytes"
    "mime/multipart"
    "os"
)

func main() {
    var b bytes.Buffer
    w := multipart.NewWriter(&b)
    // Add a file part
    fw, _ := w.CreateFormFile("filefield", "filename.txt")
    fw.Write([]byte("file content"))
    // Add a field
    w.WriteField("field1", "value1")
    w.Close()
    os.Stdout.Write(b.Bytes())
}
```

## Example: Reading Multipart Data (Reader, NextPart, Part)
```go
package main
import (
    "bytes"
    "fmt"
    "io"
    "mime/multipart"
)

func main() {
    body := `--boundary
Content-Disposition: form-data; name="field1"

value1
--boundary--
`
    r := multipart.NewReader(bytes.NewBufferString(body), "boundary")
    for {
        part, err := r.NextPart()
        if err == io.EOF {
            break
        }
        fmt.Printf("Part: %s\n", part.FormName())
    }
}
```

See the [official documentation](https://pkg.go.dev/mime/multipart) for more details.

# Go mime Package: Common Functions and Examples

The `mime` package implements MIME media types and encoding.

## Main Functions and Types
- `mime.TypeByExtension(ext string) string`: Returns the MIME type associated with the file extension.
- `mime.ExtensionsByType(typ string) ([]string, error)`: Returns the extensions known to be associated with the MIME type.
- `mime.AddExtensionType(ext, typ string) error`: Associates the extension with the MIME type.
- `mime.FormatMediaType(t string, param map[string]string) (string, error)`: Formats a media type.
- `mime.ParseMediaType(v string) (mediatype string, params map[string]string, err error)`: Parses a media type value.

## Example: Using MIME Types
```go
package main
import (
    "mime"
    "fmt"
)

func main() {
    t := mime.TypeByExtension(".html")
    fmt.Println("MIME type for .html:", t)
}
```

## Example: ExtensionsByType
```go
package main
import (
    "mime"
    "fmt"
)

func main() {
    exts, _ := mime.ExtensionsByType("text/html")
    fmt.Println("Extensions for text/html:", exts)
}
```

## Example: AddExtensionType
```go
package main
import (
    "mime"
    "fmt"
)

func main() {
    mime.AddExtensionType(".foo", "application/x-foo")
    fmt.Println(mime.TypeByExtension(".foo"))
}
```

## Example: FormatMediaType
```go
package main
import (
    "mime"
    "fmt"
)

func main() {
    s, _ := mime.FormatMediaType("text/html", map[string]string{"charset": "utf-8"})
    fmt.Println(s)
}
```

## Example: ParseMediaType
```go
package main
import (
    "mime"
    "fmt"
)

func main() {
    mt, params, _ := mime.ParseMediaType("text/html; charset=utf-8")
    fmt.Println("Media type:", mt)
    fmt.Println("Params:", params)
}
```

See the [official documentation](https://pkg.go.dev/mime) for more details.

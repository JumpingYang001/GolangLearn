# Go encoding/base64 Package: Common Functions and Examples

The `encoding/base64` package provides base64 encoding and decoding.

## Main Functions and Types
- `base64.StdEncoding`, `base64.URLEncoding`: Standard and URL-compatible encodings.
- `base64.NewEncoder(enc *base64.Encoding, w io.Writer) io.WriteCloser`: Returns a new base64 stream encoder.
- `base64.NewDecoder(enc *base64.Encoding, r io.Reader) io.Reader`: Returns a new base64 stream decoder.
- `base64.StdEncoding.EncodeToString(src []byte) string`: Encodes bytes to a base64 string.
- `base64.StdEncoding.DecodeString(s string) ([]byte, error)`: Decodes a base64 string.
- `type base64.Encoding`: Represents a base64 encoding.

## Example: Encoding and Decoding Base64
```go
package main
import (
    "encoding/base64"
    "fmt"
)

func main() {
    data := "hello world"
    encoded := base64.StdEncoding.EncodeToString([]byte(data))
    fmt.Println("Encoded:", encoded)

    decoded, err := base64.StdEncoding.DecodeString(encoded)
    if err != nil {
        panic(err)
    }
    fmt.Println("Decoded:", string(decoded))
}
```

## Example: Using base64.URLEncoding
```go
package main
import (
    "encoding/base64"
    "fmt"
)

func main() {
    data := []byte("foo@bar.com")
    encoded := base64.URLEncoding.EncodeToString(data)
    fmt.Println("URL Encoded:", encoded)
    decoded, _ := base64.URLEncoding.DecodeString(encoded)
    fmt.Println("URL Decoded:", string(decoded))
}
```

## Example: Using base64.NewEncoder and base64.NewDecoder
```go
package main
import (
    "encoding/base64"
    "os"
)

func main() {
    enc := base64.NewEncoder(base64.StdEncoding, os.Stdout)
    enc.Write([]byte("streaming"))
    enc.Close()
    // Output: c3RyZWFtaW5n
}
```

## Example: Using base64.Encoding
```go
package main
import (
    "encoding/base64"
    "fmt"
)

func main() {
    custom := base64.NewEncoding("ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_")
    encoded := custom.EncodeToString([]byte("custom encoding!"))
    fmt.Println("Custom Encoded:", encoded)
}
```

See the [official documentation](https://pkg.go.dev/encoding/base64) for more details.

# Go crypto/sha256 Package: Common Functions and Examples

The `crypto/sha256` package implements the SHA224 and SHA256 hash algorithms.

## Main Functions and Types
- `sha256.New() hash.Hash`: Returns a new hash.Hash computing the SHA256 checksum.
- `sha256.Sum256(data []byte) [32]byte`: Returns the SHA256 checksum of the data.
- `type hash.Hash`: The generic hash interface implemented by SHA256.

## Example: Generating a SHA256 Hash
```go
package main
import (
    "crypto/sha256"
    "fmt"
)

func main() {
    data := []byte("hello world")
    sum := sha256.Sum256(data)
    fmt.Printf("SHA256: %x\n", sum)
}
```

## Example: Using sha256.New and hash.Hash
```go
package main
import (
    "crypto/sha256"
    "fmt"
    "io"
)

func main() {
    h := sha256.New()
    io.WriteString(h, "hello world")
    sum := h.Sum(nil)
    fmt.Printf("SHA256 (via hash.Hash): %x\n", sum)
}
```

See the [official documentation](https://pkg.go.dev/crypto/sha256) for more details.

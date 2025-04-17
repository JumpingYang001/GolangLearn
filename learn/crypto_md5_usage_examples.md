# Go crypto/md5 Package: Common Functions and Examples

The `crypto/md5` package implements the MD5 hash algorithm.

## Main Functions and Types
- `md5.New() hash.Hash`: Returns a new hash.Hash computing the MD5 checksum.
- `md5.Sum(data []byte) [16]byte`: Returns the MD5 checksum of the data.
- `type hash.Hash`: The generic hash interface implemented by MD5.

## Example: Generating an MD5 Hash
```go
package main
import (
    "crypto/md5"
    "fmt"
)

func main() {
    data := []byte("hello world")
    sum := md5.Sum(data)
    fmt.Printf("MD5: %x\n", sum)
}
```

## Example: Using md5.New and hash.Hash
```go
package main
import (
    "crypto/md5"
    "fmt"
    "io"
)

func main() {
    h := md5.New()
    io.WriteString(h, "hello world")
    sum := h.Sum(nil)
    fmt.Printf("MD5 (via hash.Hash): %x\n", sum)
}
```

See the [official documentation](https://pkg.go.dev/crypto/md5) for more details.

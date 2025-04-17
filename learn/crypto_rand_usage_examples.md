# Go crypto/rand Package: Common Functions and Examples

The `crypto/rand` package provides cryptographically secure random number generation.

## Main Functions and Types
- `rand.Read(b []byte) (n int, err error)`: Fills the byte slice with secure random data.
- `rand.Reader io.Reader`: A global, shared cryptographically secure random source.

## Example: Generating Random Bytes
```go
package main
import (
    "crypto/rand"
    "fmt"
)

func main() {
    b := make([]byte, 8)
    _, err := rand.Read(b)
    if err != nil {
        panic(err)
    }
    fmt.Printf("Random bytes: %x\n", b)
}
```

See the [official documentation](https://pkg.go.dev/crypto/rand) for more details.

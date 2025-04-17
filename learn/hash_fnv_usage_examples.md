# Go hash/fnv Package: Common Functions and Examples

The `hash/fnv` package implements the FNV-1 and FNV-1a non-cryptographic hash functions.

## Main Functions and Types
- `fnv.New32() hash.Hash32`: Returns a new 32-bit FNV-1 hash.
- `fnv.New32a() hash.Hash32`: Returns a new 32-bit FNV-1a hash.
- `fnv.New64() hash.Hash64`: Returns a new 64-bit FNV-1 hash.
- `fnv.New64a() hash.Hash64`: Returns a new 64-bit FNV-1a hash.
- `type hash.Hash32`, `type hash.Hash64`: Interfaces for 32- and 64-bit hash functions.

## Example: Using FNV-1 32-bit Hash
```go
package main
import (
    "hash/fnv"
    "fmt"
)

func main() {
    h := fnv.New32()
    h.Write([]byte("foo"))
    fmt.Printf("FNV-1 32: %x\n", h.Sum32())
}
```

## Example: Using FNV-1a 32-bit Hash
```go
package main
import (
    "hash/fnv"
    "fmt"
)

func main() {
    h := fnv.New32a()
    h.Write([]byte("bar"))
    fmt.Printf("FNV-1a 32: %x\n", h.Sum32())
}
```

## Example: Using FNV-1 64-bit Hash
```go
package main
import (
    "hash/fnv"
    "fmt"
)

func main() {
    h := fnv.New64()
    h.Write([]byte("baz"))
    fmt.Printf("FNV-1 64: %x\n", h.Sum64())
}
```

## Example: Using FNV-1a 64-bit Hash
```go
package main
import (
    "hash/fnv"
    "fmt"
)

func main() {
    h := fnv.New64a()
    h.Write([]byte("hello world"))
    fmt.Printf("FNV-1a 64: %x\n", h.Sum64())
}
```

See the [official documentation](https://pkg.go.dev/hash/fnv) for more details.

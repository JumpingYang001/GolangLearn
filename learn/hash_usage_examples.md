# Go hash Package: Common Functions and Examples

The `hash` package provides interfaces for hash functions.

## Main Functions and Types
- `type hash.Hash`: The main interface for hash functions.
- `type hash.Hash32`, `type hash.Hash64`: Interfaces for 32- and 64-bit hash functions.
- `(*hash.Hash).Write(p []byte) (n int, err error)`: Adds data to the running hash.
- `(*hash.Hash).Sum(b []byte) []byte`: Appends the current hash to b and returns the resulting slice.
- `(*hash.Hash).Reset()`: Resets the Hash to its initial state.
- `(*hash.Hash).Size() int`: Returns the number of bytes Sum will return.
- `(*hash.Hash).BlockSize() int`: Returns the hash's underlying block size.

## Example: Using hash.Hash with hash/fnv
```go
package main
import (
    "hash/fnv"
    "fmt"
)

func main() {
    h := fnv.New32a()
    h.Write([]byte("hello"))
    fmt.Printf("FNV-1a: %x\n", h.Sum32())
}
```

## Example: Using hash.Hash32 and hash.Hash64
```go
package main
import (
    "hash/fnv"
    "fmt"
)

func main() {
    var h32 fnv.Hash32 = fnv.New32()
    var h64 fnv.Hash64 = fnv.New64()
    h32.Write([]byte("foo"))
    h64.Write([]byte("bar"))
    fmt.Println("Hash32:", h32.Sum32())
    fmt.Println("Hash64:", h64.Sum64())
}
```

## Example: Using hash.Hash Methods
```go
package main
import (
    "hash/fnv"
    "fmt"
)

func main() {
    h := fnv.New32a()
    h.Write([]byte("abc"))
    sum := h.Sum(nil)
    fmt.Printf("Sum: %x\n", sum)
    fmt.Println("Size:", h.Size())
    fmt.Println("BlockSize:", h.BlockSize())
    h.Reset()
    fmt.Println("After reset, Size:", h.Size())
}
```

See the [official documentation](https://pkg.go.dev/hash) for more details.

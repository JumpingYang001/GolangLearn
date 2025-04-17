# Go hash/crc32 Package: Common Functions and Examples

The `hash/crc32` package implements the 32-bit cyclic redundancy check, or CRC-32, checksum.

## Main Functions and Types
- `crc32.ChecksumIEEE(data []byte) uint32`: Returns the CRC-32 checksum using the IEEE polynomial.
- `crc32.New(tab *crc32.Table) hash.Hash32`: Returns a new hash.Hash32 computing the CRC-32 checksum.
- `crc32.MakeTable(poly uint32) *crc32.Table`: Creates a new polynomial table.
- `crc32.Update(crc uint32, tab *crc32.Table, p []byte) uint32`: Updates a running CRC-32 checksum.
- `crc32.IEEETable`: The default IEEE polynomial table.
- `type crc32.Table`: Represents a CRC-32 polynomial table.

## Example: Calculating CRC32
```go
package main
import (
    "hash/crc32"
    "fmt"
)

func main() {
    data := []byte("hello world")
    checksum := crc32.ChecksumIEEE(data)
    fmt.Printf("CRC32: %08x\n", checksum)
}
```

## Example: Using crc32.New and crc32.Table
```go
package main
import (
    "hash/crc32"
    "fmt"
)

func main() {
    table := crc32.MakeTable(crc32.IEEE)
    h := crc32.New(table)
    h.Write([]byte("foo"))
    fmt.Printf("CRC32 (New): %08x\n", h.Sum32())
}
```

## Example: Using crc32.Update
```go
package main
import (
    "hash/crc32"
    "fmt"
)

func main() {
    table := crc32.MakeTable(crc32.IEEE)
    crc := uint32(0)
    crc = crc32.Update(crc, table, []byte("bar"))
    fmt.Printf("CRC32 (Update): %08x\n", crc)
}
```

See the [official documentation](https://pkg.go.dev/hash/crc32) for more details.

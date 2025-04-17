# Go sync/atomic Package: Common Functions and Examples

The `sync/atomic` package provides low-level atomic memory primitives for synchronizing access to integers and pointers.

## Main Functions and Types
- `atomic.AddInt32(addr *int32, delta int32) int32`: Atomically adds delta to *addr.
- `atomic.LoadInt32(addr *int32) int32`: Atomically loads *addr.
- `atomic.StoreInt32(addr *int32, val int32)`: Atomically stores val into *addr.
- `atomic.CompareAndSwapInt32(addr *int32, old, new int32) bool`: Atomically compares and swaps.
- `atomic.Value`: A type-safe wrapper for atomic loads and stores of a value.

## Example: Atomic Operations
```go
package main
import (
    "sync/atomic"
    "fmt"
)

func main() {
    var n int32 = 0
    atomic.AddInt32(&n, 1)
    fmt.Println("n:", n)
}
```

See the [official documentation](https://pkg.go.dev/sync/atomic) for more details.

# Go container/ring Package: Common Functions and Examples

The `container/ring` package provides a circular list implementation.

## Main Functions and Types
- `ring.New(n int) *ring.Ring`: Creates a new ring of n elements.
- `(*ring.Ring).Next() *ring.Ring`: Moves to the next ring element.
- `(*ring.Ring).Prev() *ring.Ring`: Moves to the previous ring element.
- `(*ring.Ring).Do(f func(interface{}))`: Calls f for each element.
- `(*ring.Ring).Link(s *ring.Ring) *ring.Ring`: Links two rings.
- `(*ring.Ring).Unlink(n int) *ring.Ring`: Removes n elements from the ring.
- `type ring.Ring`: The ring type itself.

## Example: Using a Ring
```go
package main
import (
    "container/ring"
    "fmt"
)

func main() {
    r := ring.New(3)
    for i := 1; i <= 3; i++ {
        r.Value = i
        r = r.Next()
    }
    r.Do(func(p interface{}) {
        fmt.Println(p)
    })
}
```

## Example: Next, Prev, Link, Unlink
```go
package main
import (
    "container/ring"
    "fmt"
)

func main() {
    r1 := ring.New(2)
    r2 := ring.New(2)
    r1.Value = "A"
    r1.Next().Value = "B"
    r2.Value = "C"
    r2.Next().Value = "D"
    r1.Link(r2) // Link r2 after r1
    r := r1.Unlink(1) // Remove one element after r1
    fmt.Println("Unlinked:", r.Value) // Output: Unlinked: B
    fmt.Println("Prev of r1:", r1.Prev().Value) // Output: Prev of r1: D
    r1.Do(func(v interface{}) { fmt.Print(v, " ") }) // Output: A C D
}
```

See the [official documentation](https://pkg.go.dev/container/ring) for more details.

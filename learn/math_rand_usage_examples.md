# Go math/rand Package: Common Functions and Examples

The `math/rand` package provides pseudo-random number generation.

## Main Functions and Types
- `rand.Int() int`: Returns a non-negative pseudo-random int.
- `rand.Intn(n int) int`: Returns a non-negative pseudo-random int less than n.
- `rand.Float64() float64`: Returns a pseudo-random float64 in [0.0, 1.0).
- `rand.Seed(seed int64)`: Sets the seed for the random number generator.
- `type rand.Rand`: A source of random numbers with its own state.

## Example: Generating Random Numbers
```go
package main
import (
    "math/rand"
    "fmt"
)

func main() {
    fmt.Println("Random int:", rand.Intn(100))
}
```

## Example: rand.Int
```go
package main
import (
    "math/rand"
    "fmt"
)

func main() {
    fmt.Println("Random int:", rand.Int())
}
```

## Example: rand.Float64
```go
package main
import (
    "math/rand"
    "fmt"
)

func main() {
    fmt.Println("Random float64:", rand.Float64())
}
```

## Example: rand.Seed
```go
package main
import (
    "math/rand"
    "fmt"
    "time"
)

func main() {
    rand.Seed(time.Now().UnixNano())
    fmt.Println("Seeded random int:", rand.Intn(100))
}
```

## Example: rand.Rand
```go
package main
import (
    "math/rand"
    "fmt"
    "time"
)

func main() {
    src := rand.NewSource(time.Now().UnixNano())
    r := rand.New(src)
    fmt.Println("rand.Rand Intn:", r.Intn(100))
}
```

See the [official documentation](https://pkg.go.dev/math/rand) for more details.

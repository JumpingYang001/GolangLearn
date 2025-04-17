# Go math/big Package: Common Functions and Examples

The `math/big` package provides arbitrary-precision arithmetic (big numbers).

## Main Functions and Types
- `big.NewInt(x int64) *big.Int`: Creates a new big integer.
- `big.NewFloat(f float64) *big.Float`: Creates a new big float.
- `big.NewRat(a, b int64) *big.Rat`: Creates a new rational number.
- `(*big.Int).Add`, `Sub`, `Mul`, `Div`, etc.: Arithmetic operations for big integers.
- `(*big.Float).Add`, `Sub`, `Mul`, `Quo`, etc.: Arithmetic operations for big floats.
- `(*big.Rat).Add`, `Sub`, `Mul`, `Quo`, etc.: Arithmetic operations for rationals.
- `type big.Int`, `type big.Float`, `type big.Rat`: Types for big numbers.

## Example: Using big.Int
```go
package main
import (
    "math/big"
    "fmt"
)

func main() {
    a := big.NewInt(1)
    b := big.NewInt(2)
    c := new(big.Int)
    c.Add(a, b)
    fmt.Println("1 + 2 =", c)
}
```

## Example: Using big.Float
```go
package main
import (
    "math/big"
    "fmt"
)

func main() {
    a := big.NewFloat(1.5)
    b := big.NewFloat(2.5)
    c := new(big.Float)
    c.Add(a, b)
    fmt.Println("1.5 + 2.5 =", c)
}
```

## Example: Using big.Rat
```go
package main
import (
    "math/big"
    "fmt"
)

func main() {
    a := big.NewRat(1, 3)
    b := big.NewRat(2, 3)
    c := new(big.Rat)
    c.Add(a, b)
    fmt.Println("1/3 + 2/3 =", c)
}
```

See the [official documentation](https://pkg.go.dev/math/big) for more details.

# Go math Package: Common Functions and Examples

The `math` package provides basic constants and mathematical functions.

## Main Functions and Types
- `math.Abs(x float64) float64`: Absolute value.
- `math.Pow(x, y float64) float64`: x raised to the power y.
- `math.Sqrt(x float64) float64`: Square root.
- `math.Sin`, `math.Cos`, `math.Tan`: Trigonometric functions.
- `math.Log`, `math.Exp`: Logarithmic and exponential functions.
- `math.Max`, `math.Min`: Maximum and minimum.
- `math.Pi`, `math.E`: Mathematical constants.

## Example: Using math Functions
```go
package main
import (
    "math"
    "fmt"
)

func main() {
    fmt.Println("Abs(-5):", math.Abs(-5))
    fmt.Println("Pow(2, 3):", math.Pow(2, 3))
    fmt.Println("Sqrt(16):", math.Sqrt(16))
    fmt.Println("Sin(0):", math.Sin(0))
    fmt.Println("Cos(0):", math.Cos(0))
    fmt.Println("Tan(0):", math.Tan(0))
    fmt.Println("Log(1):", math.Log(1))
    fmt.Println("Exp(1):", math.Exp(1))
    fmt.Println("Max(1, 2):", math.Max(1, 2))
    fmt.Println("Min(1, 2):", math.Min(1, 2))
    fmt.Println("Pi:", math.Pi)
    fmt.Println("E:", math.E)
}
```

See the [official documentation](https://pkg.go.dev/math) for more details.

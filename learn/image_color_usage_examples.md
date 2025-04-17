# Go image/color Package: Common Functions and Examples

The `image/color` package provides basic color types and interfaces.

## Main Functions and Types
- `color.RGBA`, `color.NRGBA`, `color.Gray`, etc.: Common color types.
- `color.RGBA{R, G, B, A}`: Creates an RGBA color.
- `color.Gray{Y}`: Creates a grayscale color.
- `type color.Color`: The main color interface.
- `color.Model`: Converts colors to a specific model.

## Example: Using Colors
```go
package main
import (
    "image/color"
    "fmt"
)

func main() {
    c := color.RGBA{R: 255, G: 0, B: 0, A: 255}
    fmt.Println(c)
}
```

## Example: Using color.Gray
```go
package main
import (
    "image/color"
    "fmt"
)

func main() {
    g := color.Gray{Y: 128}
    fmt.Println(g)
}
```

## Example: Using color.Color and color.Model
```go
package main
import (
    "image/color"
    "fmt"
)

func main() {
    var c color.Color = color.NRGBA{R: 10, G: 20, B: 30, A: 255}
    m := color.RGBAModel
    converted := m.Convert(c)
    fmt.Println("Converted color:", converted)
}
```

See the [official documentation](https://pkg.go.dev/image/color) for more details.

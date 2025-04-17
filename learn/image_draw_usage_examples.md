# Go image/draw Package: Common Functions and Examples

The `image/draw` package provides image composition functions.

## Main Functions and Types
- `draw.Draw(dst Image, r Rectangle, src Image, sp Point, op Op)`: Draws a source image onto a destination image.
- `draw.DrawMask(dst Image, r Rectangle, src Image, sp Point, mask Image, mp Point, op Op)`: Draws with a mask.
- `type draw.Image`: The main interface for images that can be drawn to.
- `type draw.Op`: Drawing operator (e.g., Over, Src).

## Example: Drawing Images
```go
package main
import (
    "image"
    "image/color"
    "image/draw"
)

func main() {
    dst := image.NewRGBA(image.Rect(0, 0, 100, 100))
    src := image.NewUniform(color.RGBA{255, 0, 0, 255})
    draw.Draw(dst, dst.Bounds(), src, image.Point{}, draw.Src)
}
```

## Example: Drawing with a Mask
```go
package main
import (
    "image"
    "image/color"
    "image/draw"
)

func main() {
    dst := image.NewRGBA(image.Rect(0, 0, 100, 100))
    src := image.NewUniform(color.RGBA{0, 255, 0, 255})
    mask := image.NewUniform(color.Alpha{128})
    draw.DrawMask(dst, dst.Bounds(), src, image.Point{}, mask, image.Point{}, draw.Over)
}
```

## Example: Using draw.Image and draw.Op
```go
package main
import (
    "image"
    "image/color"
    "image/draw"
    "fmt"
)

func main() {
    var img draw.Image = image.NewRGBA(image.Rect(0, 0, 10, 10))
    op := draw.Over
    img.Set(0, 0, color.RGBA{0, 0, 255, 255})
    fmt.Println("draw.Image and draw.Op used:", img, op)
}
```

See the [official documentation](https://pkg.go.dev/image/draw) for more details.

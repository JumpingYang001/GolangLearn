# Go image Package: Common Functions and Examples

The `image` package provides basic 2-D image libraries.

## Main Functions and Types
- `image.NewRGBA(r image.Rectangle) *image.RGBA`: Creates a new RGBA image.
- `image.NewGray(r image.Rectangle) *image.Gray`: Creates a new grayscale image.
- `image.Rect(x0, y0, x1, y1) image.Rectangle`: Returns a rectangle.
- `image.Decode(r io.Reader) (image.Image, string, error)`: Decodes an image from a reader.
- `type image.Image`: The main image interface.
- `type image.Rectangle`: Represents a rectangle.
- `type image.Color`: The color interface.

## Example: Creating and Manipulating Images
```go
package main
import (
    "image"
    "fmt"
)

func main() {
    img := image.NewRGBA(image.Rect(0, 0, 100, 50))
    fmt.Println("Bounds:", img.Bounds())
    img.Set(0, 0, image.Black)
}
```

## Example: Creating a Grayscale Image
```go
package main
import (
    "image"
    "fmt"
)

func main() {
    img := image.NewGray(image.Rect(0, 0, 10, 10))
    fmt.Println("Gray image bounds:", img.Bounds())
}
```

## Example: Using image.Rect
```go
package main
import (
    "image"
    "fmt"
)

func main() {
    r := image.Rect(10, 20, 30, 40)
    fmt.Println("Rectangle:", r)
}
```

## Example: Decoding an Image
```go
package main
import (
    "image"
    "os"
    "log"
)

func main() {
    f, err := os.Open("test.png")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    img, format, err := image.Decode(f)
    if err != nil {
        log.Fatal(err)
    }
    _ = img // use the decoded image
    _ = format // e.g., "png"
}
```

## Example: Using image.Image, image.Rectangle, and image.Color
```go
package main
import (
    "image"
    "image/color"
    "fmt"
)

func main() {
    var img image.Image = image.NewRGBA(image.Rect(0, 0, 5, 5))
    var c color.Color = color.RGBA{255, 0, 0, 255}
    var r image.Rectangle = img.Bounds()
    fmt.Println("Image bounds:", r)
    fmt.Println("Color RGBA:", c)
}
```

See the [official documentation](https://pkg.go.dev/image) for more details.

# Go image/png Package: Common Functions and Examples

The `image/png` package provides functions for encoding and decoding PNG images.

## Main Functions and Types
- `png.Decode(r io.Reader) (image.Image, error)`: Decodes a PNG image from a reader.
- `png.Encode(w io.Writer, m image.Image) error`: Encodes an image to PNG format.

## Example: Encoding and Decoding PNG
```go
package main
import (
    "image"
    "image/color"
    "image/png"
    "os"
    "log"
)

func main() {
    img := image.NewRGBA(image.Rect(0, 0, 100, 50))
    img.Set(0, 0, color.RGBA{255, 0, 0, 255})
    f, err := os.Create("out.png")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    png.Encode(f, img)
}
```

## Example: Decoding PNG
```go
package main
import (
    "image/png"
    "os"
    "log"
)

func main() {
    f, err := os.Open("out.png")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    img, err := png.Decode(f)
    if err != nil {
        log.Fatal(err)
    }
    _ = img // use the decoded image
}
```

See the [official documentation](https://pkg.go.dev/image/png) for more details.

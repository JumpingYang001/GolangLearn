# Go image/jpeg Package: Common Functions and Examples

The `image/jpeg` package provides functions for encoding and decoding JPEG images.

## Main Functions and Types
- `jpeg.Decode(r io.Reader) (image.Image, error)`: Decodes a JPEG image from a reader.
- `jpeg.Encode(w io.Writer, m image.Image, o *jpeg.Options) error`: Encodes an image to JPEG format.
- `type jpeg.Options`: Options for encoding, such as quality.

## Example: Encoding JPEG
```go
package main
import (
    "image"
    "image/color"
    "image/jpeg"
    "os"
    "log"
)

func main() {
    img := image.NewRGBA(image.Rect(0, 0, 100, 50))
    img.Set(0, 0, color.RGBA{0, 255, 0, 255})
    f, err := os.Create("out.jpg")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    jpeg.Encode(f, img, nil)
}
```

## Example: Decoding JPEG
```go
package main
import (
    "image/jpeg"
    "os"
    "log"
)

func main() {
    f, err := os.Open("out.jpg")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    img, err := jpeg.Decode(f)
    if err != nil {
        log.Fatal(err)
    }
    _ = img // use the decoded image
}
```

## Example: Using jpeg.Options
```go
package main
import (
    "image"
    "image/color"
    "image/jpeg"
    "os"
    "log"
)

func main() {
    img := image.NewRGBA(image.Rect(0, 0, 100, 50))
    img.Set(0, 0, color.RGBA{0, 0, 255, 255})
    f, err := os.Create("out_quality.jpg")
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close()
    opts := &jpeg.Options{Quality: 80}
    jpeg.Encode(f, img, opts)
}
```

See the [official documentation](https://pkg.go.dev/image/jpeg) for more details.

# Go encoding/xml Package: Common Functions and Examples

The `encoding/xml` package provides functions for encoding and decoding XML data.

## Main Functions and Types
- `xml.Marshal(v interface{}) ([]byte, error)`: Encodes a value to XML.
- `xml.Unmarshal(data []byte, v interface{}) error`: Decodes XML data into a value.
- `xml.NewEncoder(w io.Writer) *xml.Encoder`: Returns a new XML stream encoder.
- `xml.NewDecoder(r io.Reader) *xml.Decoder`: Returns a new XML stream decoder.
- `type xml.Encoder`, `type xml.Decoder`: Streaming encoder/decoder types.
- `type xml.Token`, `type xml.StartElement`, etc.: Types for low-level XML processing.

## Example: Encoding and Decoding XML
```go
package main
import (
    "encoding/xml"
    "fmt"
)

type Note struct {
    To   string `xml:"to"`
    From string `xml:"from"`
    Body string `xml:"body"`
}

func main() {
    n := Note{To: "Alice", From: "Bob", Body: "Hello!"}
    data, _ := xml.Marshal(n)
    fmt.Println(string(data))

    var n2 Note
    xml.Unmarshal(data, &n2)
    fmt.Printf("Decoded: %+v\n", n2)
}
```

See the [official documentation](https://pkg.go.dev/encoding/xml) for more details.

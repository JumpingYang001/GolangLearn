# Go json Package: Common Functions and Examples

The `encoding/json` package provides functions for encoding and decoding JSON data.

## Main Functions and Types
- `json.Marshal(v interface{}) ([]byte, error)`: Encodes a value to JSON.
- `json.Unmarshal(data []byte, v interface{}) error`: Decodes JSON data into a value.
- `json.NewEncoder(w io.Writer) *json.Encoder`: Returns a new JSON stream encoder.
- `json.NewDecoder(r io.Reader) *json.Decoder`: Returns a new JSON stream decoder.
- `type json.Encoder`, `type json.Decoder`: Streaming encoder/decoder types.
- `type json.RawMessage`: For deferred JSON decoding.

## Example: Encoding and Decoding JSON
```go
import (
    "encoding/json"
    "fmt"
)

type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

// 1. Marshal (Encode Go value to JSON)
p := Person{Name: "Alice", Age: 30}
data, err := json.Marshal(p)
if err != nil {
    panic(err)
}
fmt.Println(string(data)) // Output: {"name":"Alice","age":30}

// 2. Unmarshal (Decode JSON to Go value)
jsonStr := `{"name":"Bob","age":25}`
var p Person
err = json.Unmarshal([]byte(jsonStr), &p)
if err != nil {
    panic(err)
}
fmt.Println(p.Name, p.Age) // Output: Bob 25

// 3. MarshalIndent (Pretty-print JSON)
p = Person{Name: "Carol", Age: 22}
data, _ = json.MarshalIndent(p, "", "  ")
fmt.Println(string(data))
// Output:
// {
//   "name": "Carol",
//   "age": 22
// }

// 4. Decoding JSON from an io.Reader
import (
    "encoding/json"
    "strings"
)

r := strings.NewReader(`{"name":"Dave","age":40}`)
dec := json.NewDecoder(r)
var p Person
dec.Decode(&p)
fmt.Println(p.Name) // Output: Dave

// 5. Encoding JSON to an io.Writer
import (
    "encoding/json"
    "os"
)

p = Person{Name: "Eve", Age: 28}
enc := json.NewEncoder(os.Stdout)
enc.Encode(p)
// Output: {"name":"Eve","age":28}

// 6. Using json.RawMessage
import (
    "encoding/json"
    "fmt"
)

type Wrapper struct {
    Data json.RawMessage `json:"data"`
}

raw := json.RawMessage([]byte(`{"foo":123}`))
w := Wrapper{Data: raw}
b, _ := json.Marshal(w)
fmt.Println(string(b)) // Output: {"data":{"foo":123}}
```

See the [official documentation](https://pkg.go.dev/encoding/json) for more details.

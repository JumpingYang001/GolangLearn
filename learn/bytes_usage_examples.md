# Go bytes Package: Common Functions and Examples

The `bytes` package in Go provides functions for manipulating byte slices and buffers. It is commonly used for efficient in-memory data processing, especially with binary or text data.

## Main Functions and Types
- `bytes.Buffer`: A growable buffer for reading and writing bytes.
- `bytes.NewBuffer(b []byte) *bytes.Buffer`: Creates a new buffer.
- `bytes.Compare`, `bytes.Equal`, `bytes.Contains`, `bytes.Index`, etc.: Byte slice comparison and search functions.
- `bytes.Split`, `bytes.Join`, `bytes.Replace`: Byte slice manipulation functions.
- `bytes.Reader`: Implements `io.Reader`, `io.Seeker`, etc. for a byte slice.

## 1. bytes.Buffer
A growable buffer for reading and writing bytes.

### Write and Read with bytes.Buffer
```go
import (
    "bytes"
    "fmt"
)

var buf bytes.Buffer
buf.WriteString("Hello, ")
buf.Write([]byte("world!"))
fmt.Println(buf.String()) // Output: Hello, world!
```

### Read from bytes.Buffer
```go
b := []byte("Go is awesome!")
buf := bytes.NewBuffer(b)
out := make([]byte, 2)
for {
    n, err := buf.Read(out)
    if n == 0 || err != nil {
        break
    }
    fmt.Print(string(out[:n]))
}
// Output: Go is awesome!
```

## 2. bytes.NewBuffer and bytes.NewBufferString
Create a buffer from a byte slice or string.
```go
buf := bytes.NewBuffer([]byte("abc"))
fmt.Println(buf.String()) // Output: abc

buf2 := bytes.NewBufferString("xyz")
fmt.Println(buf2.String()) // Output: xyz
```

## 3. bytes.Compare, bytes.Equal, bytes.Contains
Compare and search byte slices.
```go
b1 := []byte("foo")
b2 := []byte("bar")
fmt.Println(bytes.Compare(b1, b2)) // Output: 1 (b1 > b2)
fmt.Println(bytes.Equal(b1, b2))   // Output: false
fmt.Println(bytes.Contains(b1, []byte("o"))) // Output: true
```

## 4. bytes.Split, bytes.Join
Split and join byte slices.
```go
s := []byte("a,b,c")
parts := bytes.Split(s, []byte(","))

for _, part := range parts {
    fmt.Println(string(part))
}
// Output: a b c

joined := bytes.Join(parts, []byte("-"))
fmt.Println(string(joined)) // Output: a-b-c
```

## 5. bytes.Replace and bytes.Index
```go
import (
    "bytes"
    "fmt"
)
s := []byte("foo bar foo")
replaced := bytes.Replace(s, []byte("foo"), []byte("baz"), -1)
fmt.Println(string(replaced)) // Output: baz bar baz

idx := bytes.Index(s, []byte("bar"))
fmt.Println(idx) // Output: 4
```

## 6. bytes.Reader Example
```go
import (
    "bytes"
    "fmt"
    "io"
)
data := []byte("hello")
r := bytes.NewReader(data)
buf := make([]byte, 2)
for {
    n, err := r.Read(buf)
    if n == 0 || err == io.EOF {
        break
    }
    fmt.Print(string(buf[:n]))
}
// Output: hello
```

---
For more, see the [official documentation](https://pkg.go.dev/bytes).

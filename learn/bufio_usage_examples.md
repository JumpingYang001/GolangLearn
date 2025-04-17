# Go bufio Package: Common Functions and Examples

The `bufio` package in Go provides buffered I/O for reading and writing, which can improve performance and provide convenient utilities for text processing.

## Main Functions and Types
- `bufio.NewReader(rd io.Reader) *bufio.Reader`: Creates a buffered reader.
- `bufio.NewWriter(wr io.Writer) *bufio.Writer`: Creates a buffered writer.
- `bufio.NewScanner(r io.Reader) *bufio.Scanner`: Creates a scanner for reading lines or tokens.
- `bufio.NewReadWriter(rd *bufio.Reader, wr *bufio.Writer) *bufio.ReadWriter`: Combines a reader and writer.
- `(*bufio.Reader).Read`, `ReadString`, `ReadBytes`, `Peek`, etc.: Buffered reading methods.
- `(*bufio.Writer).Write`, `WriteString`, `Flush`, etc.: Buffered writing methods.
- `(*bufio.Scanner).Scan`, `Text`, `Bytes`, `Err`: Scanning methods.

## 1. bufio.Reader
Buffered reading from an `io.Reader` (like a file or string).

### Reading Lines from a File
```go
import (
    "bufio"
    "fmt"
    "os"
)

file, err := os.Open("example.txt")
if err != nil {
    panic(err)
}
defer file.Close()

scanner := bufio.NewScanner(file)
for scanner.Scan() {
    fmt.Println(scanner.Text())
}
if err := scanner.Err(); err != nil {
    fmt.Println("Error:", err)
}
```

### Reading a Line from a String
```go
import (
    "bufio"
    "strings"
)

r := strings.NewReader("hello\nworld\n")
scanner := bufio.NewScanner(r)
for scanner.Scan() {
    fmt.Println(scanner.Text())
}
```

## 2. bufio.Writer
Buffered writing to an `io.Writer` (like a file).
```go
file, _ := os.Create("output.txt")
defer file.Close()
writer := bufio.NewWriter(file)
writer.WriteString("Hello, world!\n")
writer.Flush() // Don't forget to flush!
```

## 3. bufio.ReadWriter
Combines a Reader and Writer.
```go
rw := bufio.NewReadWriter(bufio.NewReader(os.Stdin), bufio.NewWriter(os.Stdout))
// Use rw.ReadString, rw.WriteString, etc.
```

## 4. bufio.Reader Methods
- **ReadString(delim byte)**: Reads until the delimiter.
- **ReadBytes(delim byte)**: Reads bytes until the delimiter.
- **Peek(n int)**: Returns the next n bytes without advancing the reader.

## 5. bufio.Writer Methods
- **WriteString(s string)**: Writes a string.
- **Flush()**: Flushes the buffer to the underlying writer.

## 4a. bufio.Reader.ReadString Example
```go
import (
    "bufio"
    "fmt"
    "strings"
)
r := strings.NewReader("foo\nbar\nbaz\n")
reader := bufio.NewReader(r)
line, _ := reader.ReadString('\n')
fmt.Print(line) // Output: foo\n
```

## 4b. bufio.Reader.ReadBytes Example
```go
import (
    "bufio"
    "fmt"
    "strings"
)
r := strings.NewReader("abc,def,ghi,")
reader := bufio.NewReader(r)
bytes, _ := reader.ReadBytes(',')
fmt.Printf("%q", bytes) // Output: "abc,"
```

## 4c. bufio.Reader.Peek Example
```go
import (
    "bufio"
    "fmt"
    "strings"
)
r := strings.NewReader("peek example")
reader := bufio.NewReader(r)
peeked, _ := reader.Peek(4)
fmt.Printf("%s", peeked) // Output: peek
```

## 5a. bufio.Writer.Write Example
```go
import (
    "bufio"
    "os"
)
file, _ := os.Create("output2.txt")
defer file.Close()
writer := bufio.NewWriter(file)
writer.Write([]byte("Buffered Write\n"))
writer.Flush()
```

## 6. bufio.Scanner.Bytes and Err Example
```go
import (
    "bufio"
    "bytes"
    "fmt"
)
data := []byte("a\nb\nc\n")
scanner := bufio.NewScanner(bytes.NewReader(data))
for scanner.Scan() {
    fmt.Printf("%q\n", scanner.Bytes())
}
if err := scanner.Err(); err != nil {
    fmt.Println("Scan error:", err)
}
```

---
For more, see the [official documentation](https://pkg.go.dev/bufio).

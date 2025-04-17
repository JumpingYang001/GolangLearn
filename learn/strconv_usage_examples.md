# Go strconv Package: Common Functions and Examples

The `strconv` package provides conversions to and from string representations of basic data types.

## Main Functions and Types
- `strconv.Itoa(i int) string`: Converts an int to a string.
- `strconv.Atoi(s string) (int, error)`: Converts a string to an int.
- `strconv.FormatInt(i int64, base int) string`: Converts an int64 to a string in the given base.
- `strconv.ParseInt(s string, base, bitSize int) (int64, error)`: Parses a string as an int64.
- `strconv.FormatFloat(f float64, fmt byte, prec, bitSize int) string`: Converts a float64 to a string.
- `strconv.ParseFloat(s string, bitSize int) (float64, error)`: Parses a string as a float64.
- `strconv.FormatBool(b bool) string`: Converts a bool to a string.
- `strconv.ParseBool(str string) (bool, error)`: Parses a string as a bool.

## Example: String Conversions

### 1. String to Integer

#### strconv.Atoi
Converts a string to an int.
```go
import (
    "fmt"
    "strconv"
)

s := "123"
i, err := strconv.Atoi(s)
if err != nil {
    fmt.Println("Conversion error:", err)
}
fmt.Println(i) // Output: 123
```

#### strconv.ParseInt
Converts a string to an int64 with a given base and bit size.
```go
s := "ff"
i, err := strconv.ParseInt(s, 16, 64) // base 16, 64 bits
fmt.Println(i) // Output: 255
```

### 2. Integer to String

#### strconv.Itoa
Converts an int to a string.
```go
i := 456
s := strconv.Itoa(i)
fmt.Println(s) // Output: "456"
```

#### strconv.FormatInt
Converts an int64 to a string with a given base.
```go
n := int64(255)
s := strconv.FormatInt(n, 16)
fmt.Println(s) // Output: "ff"
```

### 3. String to Float

#### strconv.ParseFloat
Converts a string to a float64.
```go
s := "3.14"
f, err := strconv.ParseFloat(s, 64)
fmt.Println(f) // Output: 3.14
```

### 4. Float to String

#### strconv.FormatFloat
Converts a float64 to a string.
```go
f := 2.71828
s := strconv.FormatFloat(f, 'f', 2, 64)
fmt.Println(s) // Output: "2.72"
```

### 5. String to Bool and Bool to String

#### strconv.ParseBool
```go
b, err := strconv.ParseBool("true")
fmt.Println(b) // Output: true
```

#### strconv.FormatBool
```go
s := strconv.FormatBool(false)
fmt.Println(s) // Output: "false"
```

---

See the [official documentation](https://pkg.go.dev/strconv) for more details.

# Go fmt Package: Common Functions and Examples

The `fmt` package implements formatted I/O with functions analogous to C's printf and scanf.

## Main Functions and Types
- `fmt.Print`, `fmt.Println`, `fmt.Printf`: Print to standard output.
- `fmt.Sprintf`, `fmt.Errorf`: Format and return a string or error.
- `fmt.Scan`, `fmt.Scanf`, `fmt.Scanln`: Read formatted input from standard input.
- `fmt.Fprint`, `fmt.Fprintf`, `fmt.Fprintln`: Print to an io.Writer.
- `fmt.Sscan`, `fmt.Sscanf`, `fmt.Sscanln`: Parse from a string.

## Example: Printing and Formatting

### fmt.Println
Prints arguments with spaces and a newline.
```go
fmt.Println("Hello, world!", 123)
// Output: Hello, world! 123
```

### fmt.Printf
Prints formatted output using verbs (like %s, %d).
```go
name := "Alice"
age := 30
fmt.Printf("Name: %s, Age: %d\n", name, age)
// Output: Name: Alice, Age: 30
```

### fmt.Fprintln
Prints to a specific io.Writer (like a file or os.Stdout).
```go
fmt.Fprintln(os.Stdout, "This goes to standard output.")
```

```go
import (
    "fmt"
    "os"
)

func main() {
    file, err := os.Create("output.txt")
    if err != nil {
        panic(err)
    }
    defer file.Close()

    fmt.Fprintln(file, "This goes to a file.")
}
```

## 2. Formatting Strings (No Output)

### fmt.Sprintf
Returns a formatted string (does not print).
```go
s := fmt.Sprintf("Pi is about %.2f", 3.14159)
fmt.Println(s)
// Output: Pi is about 3.14
```

## 3. Creating Errors

### fmt.Errorf
Creates a formatted error value.
```go
err := fmt.Errorf("failed to open file: %s", filename)
```

## 4. Scanning Input

### fmt.Scanln
Reads input from standard input.
```go
var name string
fmt.Print("Enter your name: ")
fmt.Scanln(&name)
fmt.Println("Hello,", name)
```

### fmt.Scan, fmt.Scanf
```go
var a int
var b string
fmt.Print("Enter a number and a word: ")
fmt.Scan(&a, &b)
fmt.Println(a, b)

fmt.Print("Enter a float: ")
var f float64
fmt.Scanf("%f", &f)
fmt.Println(f)
```

## 5. Parsing from a String

### fmt.Sscan, fmt.Sscanf, fmt.Sscanln
```go
var i int
var s string
fmt.Sscan("42 foo", &i, &s)
fmt.Println(i, s)

var x int
fmt.Sscanf("100", "%d", &x)
fmt.Println(x)

var y string
fmt.Sscanln("bar", &y)
fmt.Println(y)
```

See the [official documentation](https://pkg.go.dev/fmt) for more details.

# Go text/tabwriter Package: Common Functions and Examples

The `text/tabwriter` package in Go provides a way to align text in columns, using tab stops. It's useful for formatting tables and columnar output in the terminal or files.

## 1. Basic Usage: Aligning Columns
```go
import (
    "fmt"
    "os"
    "text/tabwriter"
)

w := tabwriter.NewWriter(os.Stdout, 0, 0, 2, ' ', 0)
fmt.Fprintln(w, "Name\tAge\tLocation")
fmt.Fprintln(w, "Alice\t30\tSeattle")
fmt.Fprintln(w, "Bob\t25\tAustin")
w.Flush()
// Output:
// Name    Age  Location
// Alice   30   Seattle
// Bob     25   Austin
```

## 2. Customizing Padding and Flags
```go
w := tabwriter.NewWriter(os.Stdout, 8, 4, 2, ' ', tabwriter.AlignRight)
fmt.Fprintln(w, "Product\tPrice\tStock")
fmt.Fprintln(w, "Book\t12.99\t100")
fmt.Fprintln(w, "Pen\t1.50\t500")
w.Flush()
```

## 3. Writing to a Buffer or File
```go
import (
    "bytes"
    "text/tabwriter"
)

var buf bytes.Buffer
w := tabwriter.NewWriter(&buf, 0, 0, 1, ' ', 0)
fmt.Fprintln(w, "A\tB\tC")
fmt.Fprintln(w, "1\t2\t3")
w.Flush()
fmt.Print(buf.String())
```

## 4. Useful Flags
- `tabwriter.AlignRight`: Right-align numeric columns.
- `tabwriter.Debug`: Print a '|' between columns (for debugging).
- `tabwriter.TabIndent`: Use tabs for indentation.

## 5. Example: Table with Debug Flag
```go
w := tabwriter.NewWriter(os.Stdout, 0, 0, 1, ' ', tabwriter.Debug)
fmt.Fprintln(w, "A\tB\tC")
fmt.Fprintln(w, "1\t2\t3")
w.Flush()
// Output will show '|' between columns for clarity.
```

---
For more, see the [official documentation](https://pkg.go.dev/text/tabwriter).

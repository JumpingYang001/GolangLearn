# Go encoding/csv Package: Common Functions and Examples

The `encoding/csv` package provides functions for reading and writing CSV (comma-separated values) files.

## Main Functions and Types
- `csv.NewReader(r io.Reader) *csv.Reader`: Creates a new CSV reader.
- `csv.NewWriter(w io.Writer) *csv.Writer`: Creates a new CSV writer.
- `(*csv.Reader).Read() ([]string, error)`: Reads one record from the CSV.
- `(*csv.Reader).ReadAll() ([][]string, error)`: Reads all records.
- `(*csv.Writer).Write(record []string) error`: Writes a single record.
- `(*csv.Writer).WriteAll(records [][]string) error`: Writes multiple records.
- `(*csv.Writer).Flush()`: Flushes any buffered data.

## Example: Reading and Writing CSV
```go
package main
import (
    "encoding/csv"
    "os"
    "fmt"
    "log"
)

func main() {
    // Writing CSV
    file, err := os.Create("test.csv")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    writer := csv.NewWriter(file)
    writer.Write([]string{"name", "age"})
    writer.Write([]string{"Alice", "30"})
    writer.Flush()

    // Reading CSV
    file, err = os.Open("test.csv")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()
    reader := csv.NewReader(file)
    records, err := reader.ReadAll()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(records)
}
```

See the [official documentation](https://pkg.go.dev/encoding/csv) for more details.

# Go database/sql Package: Common Functions and Examples

The `database/sql` package provides a generic interface around SQL (or SQL-like) databases.

## Main Functions and Types
- `sql.Open(driverName, dataSourceName string) (*sql.DB, error)`: Opens a database connection.
- `(*sql.DB).Query(query string, args ...interface{}) (*sql.Rows, error)`: Executes a query.
- `(*sql.DB).QueryRow(query string, args ...interface{}) *sql.Row`: Executes a query that is expected to return at most one row.
- `(*sql.DB).Exec(query string, args ...interface{}) (sql.Result, error)`: Executes a query without returning any rows.
- `(*sql.DB).Close() error`: Closes the database connection.
- `type sql.DB`: Represents a database handle.
- `type sql.Rows`, `type sql.Row`: Represent query results.
- `type sql.Tx`: Represents a database transaction.

## Example: Opening a Database and Querying
```go
package main
import (
    "database/sql"
    _ "github.com/mattn/go-sqlite3" // or another driver
    "fmt"
    "log"
)

func main() {
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    db.Exec("CREATE TABLE foo (id INTEGER, name TEXT)")
    db.Exec("INSERT INTO foo VALUES (?, ?)", 1, "bar")

    row := db.QueryRow("SELECT name FROM foo WHERE id = ?", 1)
    var name string
    row.Scan(&name)
    fmt.Println(name)
}
```

See the [official documentation](https://pkg.go.dev/database/sql) for more details.

# Go net/url Package: Common Functions and Examples

The `net/url` package provides URL parsing and query escaping.

## Main Functions and Types
- `url.Parse(rawurl string) (*url.URL, error)`: Parses a URL string.
- `url.ParseRequestURI(rawurl string) (*url.URL, error)`: Parses a URL for HTTP requests.
- `url.QueryEscape(s string) string`: Escapes a string for use in a URL query.
- `url.QueryUnescape(s string) (string, error)`: Unescapes a query string.
- `type url.URL`: Represents a parsed URL.
- `type url.Values`: Represents URL query parameters.

## Example: Parsing and Building URLs
```go
package main
import (
    "net/url"
    "fmt"
)

func main() {
    u, err := url.Parse("https://example.com/search?q=golang")
    if err != nil {
        panic(err)
    }
    fmt.Println("Host:", u.Host)
    fmt.Println("Query param q:", u.Query().Get("q"))
}
```

## Example: ParseRequestURI and URL Values
```go
package main
import (
    "net/url"
    "fmt"
)

func main() {
    // Parse request URI
    u, err := url.ParseRequestURI("https://example.com/path?key=value")
    if err != nil {
        panic(err)
    }
    fmt.Println("Path:", u.Path)

    // Work with URL Values
    v := url.Values{}
    v.Set("name", "John Doe")
    v.Add("item", "book")
    v.Add("item", "pen")
    fmt.Println("Encoded query:", v.Encode())
    fmt.Println("Items:", v["item"])
}
```

## Example: Query Escaping and Unescaping
```go
package main
import (
    "net/url"
    "fmt"
)

func main() {
    // Escape query parameter
    original := "hello world & more!"
    escaped := url.QueryEscape(original)
    fmt.Println("Escaped:", escaped)

    // Unescape query parameter
    unescaped, err := url.QueryUnescape(escaped)
    if err != nil {
        panic(err)
    }
    fmt.Println("Unescaped:", unescaped)
}
```

See the [official documentation](https://pkg.go.dev/net/url) for more details.

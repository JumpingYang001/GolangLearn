# Go http Package: Common Functions and Examples

The `net/http` package provides HTTP client and server implementations.

## Main Functions and Types
- `http.Get(url string) (resp *http.Response, err error)`: Makes a GET request.
- `http.Post(url, contentType string, body io.Reader) (resp *http.Response, err error)`: Makes a POST request.
- `http.ListenAndServe(addr string, handler http.Handler) error`: Starts an HTTP server.
- `http.HandleFunc(pattern string, handler func(http.ResponseWriter, *http.Request))`: Registers a handler function.
- `type http.Client`: For custom HTTP clients.
- `type http.Request`, `type http.Response`: Represent HTTP requests and responses.
- `type http.Handler`, `type http.HandlerFunc`: Interfaces for HTTP handlers.

## Example: HTTP Server and Client

### HTTP Client Usage

#### http.Get
Performs a simple HTTP GET request.
```go
resp, err := http.Get("https://example.com")
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()
body, err := io.ReadAll(resp.Body)
fmt.Println(string(body))
```

#### http.Post
Performs a simple HTTP POST request.
```go
resp, err := http.Post("https://example.com/api", "application/json", bytes.NewBuffer([]byte(`{"key":"value"}`)))
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()
```

#### http.NewRequest / http.Client.Do
For more control (headers, methods, etc.):
```go
client := &http.Client{}
req, err := http.NewRequest("PUT", "https://example.com/resource", bytes.NewBuffer([]byte(`{"name":"test"}`)))
if err != nil {
    log.Fatal(err)
}
req.Header.Set("Authorization", "Bearer TOKEN")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()
```

### HTTP Server Usage

#### http.HandleFunc and http.ListenAndServe
Create a simple HTTP server:
```go
package main
import (
    "fmt"
    "net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func main() {
    http.HandleFunc("/hello", helloHandler)
    http.ListenAndServe(":8080", nil)
}
```

## Example: Using http.Client
```go
package main
import (
    "net/http"
    "time"
)

func main() {
    client := &http.Client{Timeout: 10 * time.Second}
    resp, err := client.Get("https://example.com")
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()
}
```

## Example: Using http.Handler and http.HandlerFunc
```go
package main
import (
    "fmt"
    "net/http"
)

type myHandler struct{}

func (h *myHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Custom handler!")
}

func main() {
    http.Handle("/custom", &myHandler{})
    http.ListenAndServe(":8081", nil)
}
```

## Example: Using http.Request and http.Response
```go
package main
import (
    "fmt"
    "net/http"
)

func main() {
    resp, err := http.Get("https://example.com")
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()
    fmt.Println("Status:", resp.Status)
    fmt.Println("Request Method:", resp.Request.Method)
}
```

---
See the [official documentation](https://pkg.go.dev/net/http) for more details.

# Go context Package: Common Functions and Examples

The `context` package provides context propagation, cancellation, and deadlines for concurrent operations.

## Main Functions and Types
- `context.Background() Context`: Returns an empty root context.
- `context.TODO() Context`: Returns a context for unknown use.
- `context.WithCancel(parent Context) (Context, CancelFunc)`: Adds cancellation to a context.
- `context.WithDeadline(parent Context, d time.Time) (Context, CancelFunc)`: Adds a deadline.
- `context.WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)`: Adds a timeout.
- `context.WithValue(parent Context, key, val interface{}) Context`: Adds a value.
- `type Context`: Interface for context operations.

## Example: Basic usage
The `context` package in Go is used for controlling cancellation, timeouts, and passing request-scoped values across API boundaries and between goroutines.

### 1. Creating a Context

#### context.Background and context.TODO
```go
ctx := context.Background() // base context, never canceled
ctx2 := context.TODO()     // placeholder when unsure which context to use
```

### 2. WithCancel
Create a context that can be canceled manually.
```go
ctx, cancel := context.WithCancel(context.Background())
go func() {
    // ... do some work ...
    cancel() // cancel the context when done
}()
<-ctx.Done() // blocks until canceled
```

### 3. WithTimeout
Create a context that is automatically canceled after a timeout.
```go
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()
select {
case <-ctx.Done():
    fmt.Println("timeout or canceled")
}
```

### 4. WithValue
Pass values through a context (use sparingly).
```go
ctx := context.WithValue(context.Background(), "userID", 123)
userID := ctx.Value("userID")
fmt.Println(userID) // Output: 123
```

### 5. Using Context in HTTP Requests
```go
req, _ := http.NewRequestWithContext(ctx, http.MethodGet, "https://example.com", nil)
resp, err := http.DefaultClient.Do(req)
```

See the [official documentation](https://pkg.go.dev/context) for more details.

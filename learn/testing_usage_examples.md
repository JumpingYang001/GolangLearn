# Go testing Package: Common Functions and Examples

The `testing` package provides support for automated testing of Go packages. It's designed to work with the `go test` command and provides a rich set of tools for writing unit tests, benchmarks, and examples.

## Main Functions and Types
- `testing.T`: Test helper type for managing test state and reporting
- `testing.B`: Benchmark helper type for managing benchmark state
- `testing.M`: Test manager type for controlling test execution
- `t.Run(name string, f func(t *testing.T))`: Run a subtest
- `t.Parallel()`: Indicates that this test can run in parallel with other tests
- `t.Error/t.Errorf`: Report test failures
- `t.Fatal/t.Fatalf`: Report test failures and stop test execution
- `t.Skip/t.Skipf`: Skip this test
- `t.Cleanup(func())`: Register cleanup functions

## Basic Test Example
```go
package calculator

import "testing"

// Function to test
func Add(a, b int) int {
    return a + b
}

// Basic test function
func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5
    
    if got != want {
        t.Errorf("Add(2, 3) = %d; want %d", got, want)
    }
}

// Table-driven test
func TestAddTable(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        want     int
    }{
        {"positive numbers", 2, 3, 5},
        {"zero and positive", 0, 1, 1},
        {"negative numbers", -1, -2, -3},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.want {
                t.Errorf("Add(%d, %d) = %d; want %d", 
                    tt.a, tt.b, got, tt.want)
            }
        })
    }
}
```

## Test Setup and Teardown Example
```go
package database

import (
    "testing"
    "database/sql"
)

func TestDatabase(t *testing.T) {
    // Setup
    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatalf("Failed to open database: %v", err)
    }
    
    // Cleanup (runs after test completes)
    t.Cleanup(func() {
        db.Close()
    })
    
    // Test cases
    t.Run("insert record", func(t *testing.T) {
        // Test implementation
    })
    
    t.Run("query record", func(t *testing.T) {
        // Test implementation
    })
}
```

## Parallel Test Example
```go
package parallel

import (
    "testing"
    "time"
)

func TestParallel(t *testing.T) {
    // Top-level test runs serially
    t.Run("group", func(t *testing.T) {
        // Subtests run in parallel
        for i := 0; i < 3; i++ {
            num := i // Capture loop variable
            t.Run("parallel test "+string(num), func(t *testing.T) {
                t.Parallel() // Mark test as parallel
                time.Sleep(100 * time.Millisecond)
                // Test implementation
            })
        }
    })
}
```

## Test Main Example
```go
package main

import (
    "os"
    "testing"
)

var db *sql.DB

// TestMain is used for setup and teardown of test suite
func TestMain(m *testing.M) {
    // Setup
    var err error
    db, err = sql.Open("sqlite3", ":memory:")
    if err != nil {
        os.Exit(1)
    }
    
    // Run tests
    code := m.Run()
    
    // Cleanup
    db.Close()
    
    // Exit with test result code
    os.Exit(code)
}

// Regular tests can use the db variable
func TestDatabaseOps(t *testing.T) {
    // Use db here
}
```

## Helper Function Example
```go
package testing

import "testing"

// Helper function to set up test data
func setupTestData(t *testing.T) []string {
    t.Helper() // Mark this as a helper function
    return []string{"test", "data"}
}

func TestWithHelper(t *testing.T) {
    data := setupTestData(t)
    // Use test data
    if len(data) == 0 {
        t.Error("Test data is empty")
    }
}
```

## Benchmark Example
```go
package benchmark

import "testing"

func Fibonacci(n int) int {
    if n < 2 {
        return n
    }
    return Fibonacci(n-1) + Fibonacci(n-2)
}

func BenchmarkFibonacci(b *testing.B) {
    // Reset timer for setup
    b.ResetTimer()
    
    for i := 0; i < b.N; i++ {
        Fibonacci(10)
    }
}

// Benchmark with different input sizes
func BenchmarkFibonacciParameterized(b *testing.B) {
    benchmarks := []struct {
        name string
        n    int
    }{
        {"small", 5},
        {"medium", 10},
        {"large", 15},
    }
    
    for _, bm := range benchmarks {
        b.Run(bm.name, func(b *testing.B) {
            for i := 0; i < b.N; i++ {
                Fibonacci(bm.n)
            }
        })
    }
}
```

## Example Test (Documentation Examples)
```go
package example

import "fmt"

func ExampleHello() {
    fmt.Println("Hello, world!")
    // Output: Hello, world!
}

// Example with multiple lines of output
func ExampleGreet() {
    fmt.Println("Hello")
    fmt.Println("World")
    // Output:
    // Hello
    // World
}

// Example with unordered output
func ExampleUnordered() {
    fmt.Println("Three")
    fmt.Println("One")
    fmt.Println("Two")
    // Unordered output:
    // One
    // Two
    // Three
}
```

## Testing HTTP Handlers Example
```go
package http

import (
    "net/http"
    "net/http/httptest"
    "testing"
)

func TestHTTPHandler(t *testing.T) {
    // Create a test handler
    handler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.WriteHeader(http.StatusOK)
        w.Write([]byte("Hello, test!"))
    })
    
    // Create test request
    req := httptest.NewRequest("GET", "/test", nil)
    
    // Create response recorder
    rec := httptest.NewRecorder()
    
    // Serve the request
    handler.ServeHTTP(rec, req)
    
    // Check status code
    if rec.Code != http.StatusOK {
        t.Errorf("got status %d; want %d", rec.Code, http.StatusOK)
    }
    
    // Check response body
    if rec.Body.String() != "Hello, test!" {
        t.Errorf("got body %q; want %q", rec.Body.String(), "Hello, test!")
    }
}
```

## Important Notes
- Test files must end with `_test.go`
- Test functions must start with `Test`
- Benchmark functions must start with `Benchmark`
- Example functions must start with `Example`
- Use table-driven tests when testing multiple cases
- Use subtests (`t.Run`) for better organization and parallel execution
- Use `t.Helper()` for helper functions to get better error reporting
- Use `testing.TB` interface when writing helpers that work with both tests and benchmarks
- The testing package integrates with the `go test` command
- Use `go test -v` for verbose output
- Use `go test -run=Pattern` to run specific tests
- Use `go test -bench=.` to run benchmarks
- Use `go test -cover` to see test coverage

See the [official documentation](https://pkg.go.dev/testing) for more details.

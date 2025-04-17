# Go errors Package: Common Functions and Examples

The `errors` package provides simple error handling primitives.

## Main Functions and Types
- `errors.New(text string) error`: Creates a new error with the given text.
- `errors.Is(err, target error) bool`: Checks if an error matches a target error.
- `errors.As(err error, target interface{}) bool`: Checks if an error can be assigned to a target type.
- `errors.Unwrap(err error) error`: Returns the next error in the chain.

## Example: Creating and Wrapping Errors
```go
package main
import (
    "errors"
    "fmt"
)

func main() {
    err := errors.New("something went wrong")
    fmt.Println(err)

    wrapped := fmt.Errorf("operation failed: %w", err)
    fmt.Println(wrapped)
}
```

## Example: Using errors.Is
```go
package main
import (
    "errors"
    "fmt"
)

func main() {
    base := errors.New("base error")
    wrapped := fmt.Errorf("wrap: %w", base)
    fmt.Println(errors.Is(wrapped, base)) // true
}
```

## Example: Using errors.As
```go
package main
import (
    "errors"
    "fmt"
)

type myErr struct{}
func (m *myErr) Error() string { return "my error" }

func main() {
    var target *myErr
    err := &myErr{}
    wrapped := fmt.Errorf("wrap: %w", err)
    if errors.As(wrapped, &target) {
        fmt.Println("matched myErr type")
    }
}
```

## Example: Using errors.Unwrap
```go
package main
import (
    "errors"
    "fmt"
)

func main() {
    base := errors.New("base error")
    wrapped := fmt.Errorf("wrap: %w", base)
    unwrapped := errors.Unwrap(wrapped)
    fmt.Println(unwrapped)
}
```

See the [official documentation](https://pkg.go.dev/errors) for more details.

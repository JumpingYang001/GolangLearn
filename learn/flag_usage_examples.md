# Go flag Package: Common Functions and Examples

The `flag` package provides command-line flag parsing.

## Main Functions and Types
- `flag.Bool`, `flag.Int`, `flag.String`, etc.: Define command-line flags of various types.
- `flag.Parse()`: Parses the command-line flags.
- `flag.Args() []string`: Returns non-flag arguments.
- `flag.NArg() int`: Returns the number of non-flag arguments.
- `flag.Visit(fn func(*flag.Flag))`: Visits set flags.
- `type flag.FlagSet`: Represents a set of defined flags.

## Example: Defining and Parsing Flags
```go
package main
import (
    "flag"
    "fmt"
)

func main() {
    name := flag.String("name", "world", "a name to say hello to")
    flag.Parse()
    fmt.Printf("Hello, %s!\n", *name)
}
```

## Example: Using flag.Args and flag.NArg
```go
package main
import (
    "flag"
    "fmt"
)

func main() {
    flag.Parse()
    fmt.Println("Non-flag arguments:", flag.Args())
    fmt.Println("Number of non-flag arguments:", flag.NArg())
}
```

## Example: Using flag.Visit
```go
package main
import (
    "flag"
    "fmt"
)

func main() {
    foo := flag.Bool("foo", false, "foo flag")
    bar := flag.String("bar", "", "bar flag")
    flag.Parse()
    flag.Visit(func(f *flag.Flag) {
        fmt.Printf("Flag set: %s = %s\n", f.Name, f.Value)
    })
    _ = foo
    _ = bar
}
```

## Example: Using flag.FlagSet
```go
package main
import (
    "flag"
    "fmt"
)

func main() {
    fs := flag.NewFlagSet("example", flag.ExitOnError)
    verbose := fs.Bool("verbose", false, "enable verbose mode")
    fs.Parse([]string{"-verbose"})
    fmt.Println("Verbose:", *verbose)
}
```

See the [official documentation](https://pkg.go.dev/flag) for more details.

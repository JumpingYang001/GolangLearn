# Go plugin Package: Common Functions and Examples

The `plugin` package provides access to Go plugins (shared libraries).

## Main Functions and Types
- `plugin.Open(path string) (*plugin.Plugin, error)`: Opens a plugin.
- `(*plugin.Plugin).Lookup(symName string) (Symbol, error)`: Looks up a symbol (function or variable) in the plugin.
- `type plugin.Plugin`: Represents an open plugin.
- `type plugin.Symbol`: Represents a symbol loaded from a plugin.

## Example: Creating a Plugin
```go
// plugin/greeter.go
package main

var Greeting string = "Hello"

func SayHello(name string) {
    println(Greeting + ", " + name + " from plugin!")
}
```

## Example: Building and Loading a Plugin
```go
package main
import (
    "plugin"
    "fmt"
)

func main() {
    // First, build the plugin:
    // go build -buildmode=plugin -o greeter.so plugin/greeter.go

    // Load the plugin
    p, err := plugin.Open("greeter.so")
    if err != nil {
        fmt.Println("Error loading plugin:", err)
        return
    }

    // Look up an exported variable
    greetingVar, err := p.Lookup("Greeting")
    if err != nil {
        fmt.Println("Lookup Greeting error:", err)
        return
    }
    greeting := greetingVar.(*string)
    *greeting = "Hi there" // Modify the plugin's variable

    // Look up and call an exported function
    sayHello, err := p.Lookup("SayHello")
    if err != nil {
        fmt.Println("Lookup SayHello error:", err)
        return
    }
    fmt.Println("Symbol SayHello loaded:", sayHello)
    
    sayHello.(func(string))("Gopher") // Prints: Hi there, Gopher from plugin!

    *g.(*string) = "I am"
    sayHello.(func(string))(*g.(*string)) // prints "Hello, I am from plugin!"
}
```

## Notes
- Plugins are supported only on Linux and macOS
- Plugin must be built with same Go version as main program
- Plugin must have package main
- Use -buildmode=plugin when building

See the [official documentation](https://pkg.go.dev/plugin) for more details.

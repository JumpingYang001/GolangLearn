# Go os/signal Package: Common Functions and Examples

The `os/signal` package provides access to incoming signals.

## Main Functions and Types
- `signal.Notify(c chan<- os.Signal, sig ...os.Signal)`: Registers the channel to receive notifications of specified signals.
- `signal.Stop(c chan<- os.Signal)`: Stops signal delivery to the channel.
- `signal.Ignore(sig ...os.Signal)`: Causes the program to ignore the provided signals.
- `type os.Signal`: Interface representing an operating system signal.

## Example: Handling Signals
```go
package main
import (
    "os"
    "os/signal"
    "syscall"
    "fmt"
)

func main() {
    sigs := make(chan os.Signal, 1)
    signal.Notify(sigs, syscall.SIGINT, syscall.SIGTERM)
    fmt.Println("Waiting for signal...")
    sig := <-sigs
    fmt.Println("Received:", sig)
}
```

## Example: Using Stop
```go
package main
import (
    "os"
    "os/signal"
    "syscall"
    "fmt"
    "time"
)

func main() {
    sigs := make(chan os.Signal, 1)
    signal.Notify(sigs, syscall.SIGINT)
    
    // Stop receiving signals after 5 seconds
    go func() {
        time.Sleep(5 * time.Second)
        signal.Stop(sigs)
        fmt.Println("Stopped receiving signals")
    }()
    
    fmt.Println("Press Ctrl+C within 5 seconds...")
    sig := <-sigs
    fmt.Println("Received:", sig)
}
```

## Example: Using Ignore
```go
package main
import (
    "os"
    "os/signal"
    "syscall"
    "fmt"
    "time"
)

func main() {
    // Ignore SIGINT (Ctrl+C)
    signal.Ignore(syscall.SIGINT)
    
    fmt.Println("SIGINT is now ignored. Program will run for 10 seconds...")
    time.Sleep(10 * time.Second)
    fmt.Println("Done!")
}
```

See the [official documentation](https://pkg.go.dev/os/signal) for more details.

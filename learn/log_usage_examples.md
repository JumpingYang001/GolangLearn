# Go log Package: Common Functions and Examples

The `log` package provides simple logging capabilities.

## Main Functions and Types
- `log.Print`, `log.Println`, `log.Printf`: Print log messages.
- `log.Fatal`, `log.Fatalf`, `log.Fatalln`: Print log messages and exit.
- `log.Panic`, `log.Panicf`, `log.Panicln`: Print log messages and panic.
- `log.SetOutput(w io.Writer)`: Sets the output destination for the logger.
- `log.SetFlags(flag int)`: Sets the logging properties.
- `log.New(out io.Writer, prefix string, flag int) *log.Logger`: Creates a new logger.
- `type log.Logger`: Custom logger type.

## Example: Logging

### log.Print, log.Println
Prints a log message.
```go
import "log"

log.Print("This is a log message.")
log.Println("This is a log message with a newline.")
```

### log.Printf
Prints a formatted log message.
```go
name := "Alice"
log.Printf("Hello, %s!", name)
```

### log.Fatal, log.Fatalln, log.Fatalf
Prints a message and then calls os.Exit(1).
```go
log.Fatal("Fatal error!")
log.Fatalf("Fatal error: %s", "something went wrong")
```

### log.Panic, log.Panicln, log.Panicf
Prints a message and then panics.
```go
log.Panic("Panic error!")
log.Panicf("Panic error: %s", "something went wrong")
```

### log.SetPrefix and log.SetFlags
Set a prefix and control the log output format.
```go
log.SetPrefix("[MyApp] ")
log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)
log.Println("This is a custom log message.")
```

## 4. Logging to a File
```go
import (
    "log"
    "os"
)

file, err := os.Create("app.log")
if err != nil {
    log.Fatal(err)
}
defer file.Close()
log.SetOutput(file)
log.Println("This message goes to the file.")
```

## Example: log.New and log.Logger
```go
import (
    "log"
    "os"
)

func main() {
    logger := log.New(os.Stdout, "[Custom] ", log.LstdFlags)
    logger.Println("This is a custom logger message.")
}
```

See the [official documentation](https://pkg.go.dev/log) for more details.

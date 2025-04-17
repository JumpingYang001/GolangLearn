# Go os/exec Package: Common Functions and Examples

The `os/exec` package runs external commands.

## Main Functions and Types
- `exec.Command(name string, arg ...string) *exec.Cmd`: Prepares a command for execution.
- `(*exec.Cmd).Run() error`: Runs the command and waits for it to finish.
- `(*exec.Cmd).Output() ([]byte, error)`: Runs the command and returns its standard output.
- `(*exec.Cmd).Start() error`: Starts the command but does not wait for it.
- `(*exec.Cmd).Wait() error`: Waits for the command to exit.
- `type exec.Cmd`: Represents an external command being prepared or run.

## Example: Running External Commands
```go
package main
import (
    "os/exec"
    "fmt"
)

func main() {
    out, err := exec.Command("echo", "hello").Output()
    if err != nil {
        panic(err)
    }
    fmt.Println(string(out))
}
```

## Example: Using Start and Wait
```go
package main
import (
    "os/exec"
    "fmt"
    "time"
)

func main() {
    cmd := exec.Command("sleep", "2")
    
    if err := cmd.Start(); err != nil {
        panic(err)
    }
    
    fmt.Println("Waiting for command to finish...")
    
    if err := cmd.Wait(); err != nil {
        panic(err)
    }
    fmt.Println("Command finished")
}
```

## Example: Configuring Command Execution
```go
package main
import (
    "os/exec"
    "os"
    "fmt"
)

func main() {
    cmd := exec.Command("ls", "-l")
    
    // Set working directory
    cmd.Dir = "/tmp"
    
    // Set environment variables
    cmd.Env = append(os.Environ(), "LANG=en_US.UTF-8")
    
    // Configure I/O
    cmd.Stdout = os.Stdout
    cmd.Stderr = os.Stderr
    
    // Run command
    if err := cmd.Run(); err != nil {
        panic(err)
    }
}
```

See the [official documentation](https://pkg.go.dev/os/exec) for more details.

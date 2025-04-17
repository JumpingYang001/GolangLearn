# Go syscall Package: Common Functions and Examples

The `syscall` package provides a low-level interface to operating system primitives. It's primarily used for making direct system calls and accessing low-level operating system functionality.

## Main Functions and Types
- `syscall.Open(path string, mode int, perm uint32) (fd int, err error)`: Opens a file
- `syscall.Close(fd int) (err error)`: Closes a file descriptor
- `syscall.Read(fd int, p []byte) (n int, err error)`: Reads from a file descriptor
- `syscall.Write(fd int, p []byte) (n int, err error)`: Writes to a file descriptor
- `syscall.Chmod(path string, mode uint32) (err error)`: Changes file permissions
- `syscall.Chown(path string, uid, gid int) error`: Changes file owner and group
- `syscall.Socket(domain, typ, proto int) (fd int, err error)`: Creates an endpoint for communication
- `syscall.Kill(pid int, signal Signal) (err error)`: Sends a signal to a process

## Basic File Operations Example
```go
package main

import (
    "fmt"
    "syscall"
)

func main() {
    // Open a file
    fd, err := syscall.Open("test.txt", syscall.O_RDWR|syscall.O_CREAT, 0666)
    if err != nil {
        fmt.Println("Error opening file:", err)
        return
    }
    defer syscall.Close(fd)

    // Write to file
    message := []byte("Hello, syscall!")
    n, err := syscall.Write(fd, message)
    if err != nil {
        fmt.Println("Error writing to file:", err)
        return
    }
    fmt.Printf("Wrote %d bytes\n", n)

    // Read from file (seek to beginning first)
    _, err = syscall.Seek(fd, 0, 0)
    if err != nil {
        fmt.Println("Error seeking:", err)
        return
    }

    buf := make([]byte, 100)
    n, err = syscall.Read(fd, buf)
    if err != nil {
        fmt.Println("Error reading file:", err)
        return
    }
    fmt.Printf("Read %d bytes: %s\n", n, buf[:n])
}
```

## File Permissions Example
```go
package main

import (
    "fmt"
    "syscall"
)

func main() {
    // Create a file
    fd, err := syscall.Open("permissions.txt", syscall.O_RDWR|syscall.O_CREAT, 0666)
    if err != nil {
        fmt.Println("Error creating file:", err)
        return
    }
    syscall.Close(fd)

    // Change permissions (read/write for owner only)
    err = syscall.Chmod("permissions.txt", 0600)
    if err != nil {
        fmt.Println("Error changing permissions:", err)
        return
    }

    // Get file stats
    var stat syscall.Stat_t
    err = syscall.Stat("permissions.txt", &stat)
    if err != nil {
        fmt.Println("Error getting file stats:", err)
        return
    }
    fmt.Printf("File mode: %o\n", stat.Mode&0777)
}
```

## Process Signal Handling Example
```go
package main

import (
    "fmt"
    "os"
    "syscall"
    "time"
)

func main() {
    // Get current process ID
    pid := os.Getpid()
    fmt.Printf("Current process ID: %d\n", pid)

    // Create a signal channel
    sigChan := make(chan os.Signal, 1)
    signal.Notify(sigChan, syscall.SIGINT, syscall.SIGTERM)

    // Send SIGTERM to self after 2 seconds
    go func() {
        time.Sleep(2 * time.Second)
        err := syscall.Kill(pid, syscall.SIGTERM)
        if err != nil {
            fmt.Println("Error sending signal:", err)
        }
    }()

    // Wait for signal
    sig := <-sigChan
    fmt.Printf("Received signal: %v\n", sig)
}
```

## Socket Communication Example
```go
package main

import (
    "fmt"
    "syscall"
)

func main() {
    // Create a TCP socket
    fd, err := syscall.Socket(syscall.AF_INET, syscall.SOCK_STREAM, 0)
    if err != nil {
        fmt.Println("Error creating socket:", err)
        return
    }
    defer syscall.Close(fd)

    // Prepare the address structure
    addr := &syscall.SockaddrInet4{
        Port: 8080,
        Addr: [4]byte{127, 0, 0, 1},
    }

    // Bind the socket
    err = syscall.Bind(fd, addr)
    if err != nil {
        fmt.Println("Error binding socket:", err)
        return
    }

    // Listen for connections
    err = syscall.Listen(fd, 10)
    if err != nil {
        fmt.Println("Error listening:", err)
        return
    }
    fmt.Println("Socket listening on 127.0.0.1:8080")
}
```

## System Information Example
```go
package main

import (
    "fmt"
    "syscall"
)

func main() {
    var uname syscall.Utsname
    err := syscall.Uname(&uname)
    if err != nil {
        fmt.Println("Error getting system info:", err)
        return
    }

    // Convert byte arrays to strings
    sysname := string(uname.Sysname[:])
    nodename := string(uname.Nodename[:])
    release := string(uname.Release[:])
    version := string(uname.Version[:])
    machine := string(uname.Machine[:])

    fmt.Printf("System name: %s\n", sysname)
    fmt.Printf("Node name: %s\n", nodename)
    fmt.Printf("Release: %s\n", release)
    fmt.Printf("Version: %s\n", version)
    fmt.Printf("Machine: %s\n", machine)
}
```

## Directory Operations Example
```go
package main

import (
    "fmt"
    "syscall"
)

func main() {
    // Create a directory
    err := syscall.Mkdir("testdir", 0755)
    if err != nil {
        fmt.Println("Error creating directory:", err)
        return
    }

    // Change current working directory
    err = syscall.Chdir("testdir")
    if err != nil {
        fmt.Println("Error changing directory:", err)
        return
    }

    // Get current working directory
    buf := make([]byte, 100)
    cwd, err := syscall.Getcwd(buf)
    if err != nil {
        fmt.Println("Error getting current directory:", err)
        return
    }
    fmt.Printf("Current directory: %s\n", cwd)

    // Change back to parent directory and remove test directory
    err = syscall.Chdir("..")
    if err != nil {
        fmt.Println("Error changing directory:", err)
        return
    }

    err = syscall.Rmdir("testdir")
    if err != nil {
        fmt.Println("Error removing directory:", err)
        return
    }
}
```

## Important Notes
- Syscall operations are very low-level and can be dangerous if not used properly
- Error handling is crucial when working with system calls
- Some operations may require elevated privileges
- The syscall package is system-dependent; some calls may work differently or not at all on different operating systems
- For most common operations, it's recommended to use the higher-level packages in the standard library (like `os`, `io`, etc.)

See the [official documentation](https://pkg.go.dev/syscall) for more details.

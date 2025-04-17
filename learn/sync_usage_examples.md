# Go sync Package: Common Functions and Examples

The `sync` package provides basic synchronization primitives such as mutual exclusion locks.

## Main Functions and Types
- `sync.Mutex`: A mutual exclusion lock.
- `sync.RWMutex`: A reader/writer mutual exclusion lock.
- `sync.WaitGroup`: Waits for a collection of goroutines to finish.
- `sync.Once`: Ensures a function is only called once.
- `sync.Cond`: A condition variable for goroutine coordination.
- `sync.Map`: A concurrent map.

## Example: Using Mutex and WaitGroup
A mutual exclusion lock for protecting shared data.
```go
import (
    "fmt"
    "sync"
)

var mu sync.Mutex
var count int

func increment() {
    mu.Lock()
    count++
    mu.Unlock()
}
```

Waits for a collection of goroutines to finish.
```go
var wg sync.WaitGroup
for i := 0; i < 3; i++ {
    wg.Add(1)
    go func(i int) {
        defer wg.Done()
        fmt.Println(i)
    }(i)
}
wg.Wait()
```

See the [official documentation](https://pkg.go.dev/sync) for more details.

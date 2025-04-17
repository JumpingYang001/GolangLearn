# Go unsafe Package: Common Functions and Examples

The `unsafe` package contains operations that step around the type safety guarantees of Go. It should be used with extreme caution and only when absolutely necessary.

## Main Functions and Types
- `unsafe.Pointer`: Special type for holding arbitrary pointers
- `unsafe.Sizeof(x ArbitraryType)`: Returns size of variable in bytes
- `unsafe.Alignof(x ArbitraryType)`: Returns required alignment of variable
- `unsafe.Offsetof(x.f)`: Returns offset of struct field in bytes
- `unsafe.Add(ptr Pointer, len IntegerType)`: Adds len to ptr

## ⚠️ Important Warning
The unsafe package should be used with extreme caution. Its use:
- May break memory safety
- Makes code non-portable
- May break with different Go versions
- Can cause undefined behavior
- May not work with garbage collector

## Basic Memory Layout Example
```go
package main

import (
    "fmt"
    "unsafe"
)

type Example struct {
    A byte
    B int64
    C byte
}

func main() {
    var e Example
    
    // Size and alignment of struct and fields
    fmt.Printf("Size of struct: %d bytes\n", unsafe.Sizeof(e))
    fmt.Printf("Alignment of struct: %d bytes\n", unsafe.Alignof(e))
    
    // Field offsets
    fmt.Printf("Offset of A: %d\n", unsafe.Offsetof(e.A))
    fmt.Printf("Offset of B: %d\n", unsafe.Offsetof(e.B))
    fmt.Printf("Offset of C: %d\n", unsafe.Offsetof(e.C))
    
    // Size of different types
    fmt.Printf("\nSize of basic types:\n")
    fmt.Printf("bool:    %d bytes\n", unsafe.Sizeof(bool(true)))
    fmt.Printf("int8:    %d bytes\n", unsafe.Sizeof(int8(0)))
    fmt.Printf("int16:   %d bytes\n", unsafe.Sizeof(int16(0)))
    fmt.Printf("int32:   %d bytes\n", unsafe.Sizeof(int32(0)))
    fmt.Printf("int64:   %d bytes\n", unsafe.Sizeof(int64(0)))
    fmt.Printf("string:  %d bytes\n", unsafe.Sizeof(""))
    fmt.Printf("slice:   %d bytes\n", unsafe.Sizeof([]int{}))
    fmt.Printf("map:     %d bytes\n", unsafe.Sizeof(map[string]int{}))
    fmt.Printf("chan:    %d bytes\n", unsafe.Sizeof(make(chan int)))
}
```

## Pointer Conversion Example
```go
package main

import (
    "fmt"
    "unsafe"
)

func main() {
    // Create an integer
    x := int64(42)
    
    // Convert *int64 to unsafe.Pointer
    p := unsafe.Pointer(&x)
    
    // Convert unsafe.Pointer to *int64
    xPtr := (*int64)(p)
    
    fmt.Printf("Original value: %d\n", x)
    fmt.Printf("Through pointer: %d\n", *xPtr)
    
    // Demonstrate pointer arithmetic (be very careful!)
    arr := []int64{1, 2, 3, 4, 5}
    
    // Get pointer to first element
    firstPtr := unsafe.Pointer(&arr[0])
    
    // Move to second element
    secondPtr := unsafe.Add(firstPtr, unsafe.Sizeof(arr[0]))
    
    // Convert back to *int64
    secondEl := (*int64)(secondPtr)
    
    fmt.Printf("Second element: %d\n", *secondEl)
}
```

## Type Conversion Example
```go
package main

import (
    "fmt"
    "unsafe"
)

func main() {
    // String to bytes conversion without copy
    s := "Hello, World!"
    
    // Get string header
    stringHeader := (*struct {
        Data unsafe.Pointer
        Len  int
    })(unsafe.Pointer(&s))
    
    // Create slice header
    sliceHeader := struct {
        Data unsafe.Pointer
        Len  int
        Cap  int
    }{
        Data: stringHeader.Data,
        Len:  stringHeader.Len,
        Cap:  stringHeader.Len,
    }
    
    // Convert to byte slice without copy
    // WARNING: This is unsafe! The slice must not be modified!
    bytes := *(*[]byte)(unsafe.Pointer(&sliceHeader))
    
    fmt.Printf("String: %s\n", s)
    fmt.Printf("Bytes: %v\n", bytes)
    
    // IMPORTANT: The bytes slice shares memory with the string
    // Modifying the slice would be undefined behavior!
}
```

## Memory Access Example
```go
package main

import (
    "fmt"
    "unsafe"
)

type Data struct {
    value int64
    flag  bool
}

func main() {
    // Create a value
    d := Data{
        value: 42,
        flag:  true,
    }
    
    // Get pointer to structure
    ptr := unsafe.Pointer(&d)
    
    // Access value field directly
    valuePtr := (*int64)(ptr)
    fmt.Printf("Value: %d\n", *valuePtr)
    
    // Access flag field
    flagPtr := (*bool)(unsafe.Add(ptr, unsafe.Offsetof(d.flag)))
    fmt.Printf("Flag: %v\n", *flagPtr)
    
    // WARNING: This kind of direct memory access is dangerous
    // and should be avoided unless absolutely necessary!
}
```

## Important Notes
- Use unsafe ONLY when:
  - Performance is critical
  - Interfacing with C code
  - Implementing low-level system calls
  - Working with hardware directly
- Common unsafe.Pointer rules:
  - Can convert between unsafe.Pointer and *T
  - Can convert between unsafe.Pointer and uintptr
  - uintptr must be converted back to unsafe.Pointer immediately
  - Don't store uintptr values
- Potential issues:
  - Memory corruption
  - Race conditions
  - Garbage collection problems
  - Platform-specific behavior
  - Breaking changes in Go versions
- Better alternatives:
  - Use reflection for type manipulation
  - Use encoding/binary for byte manipulation
  - Use proper type conversions
  - Use proper memory allocation
- Documentation and review:
  - Document all unsafe usage
  - Explain why it's necessary
  - Review alternatives
  - Consider maintenance burden
  - Plan for future Go versions

See the [official documentation](https://pkg.go.dev/unsafe) for more details and complete rules for unsafe.Pointer use.

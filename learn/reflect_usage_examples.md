# Go reflect Package: Common Functions and Examples

The `reflect` package provides runtime reflection, allowing a program to manipulate objects with arbitrary types.

## Main Functions and Types
- `reflect.TypeOf(i interface{}) reflect.Type`: Returns the reflection Type of the value.
- `reflect.ValueOf(i interface{}) reflect.Value`: Returns the reflection Value of the value.
- `reflect.New(t reflect.Type) reflect.Value`: Creates a new zero Value for the type.
- `reflect.Indirect(v reflect.Value) reflect.Value`: Returns the value that v points to.
- `reflect.DeepEqual(a, b interface{}) bool`: Checks deep equality.
- `type reflect.Type`, `type reflect.Value`: Core types for reflection.

## Example: Basic Type and Value Reflection
```go
package main
import (
    "fmt"
    "reflect"
)

func main() {
    var x int = 42
    
    // Get Type information
    t := reflect.TypeOf(x)
    fmt.Printf("Type: %v, Kind: %v\n", t, t.Kind())
    
    // Get and use Value information
    v := reflect.ValueOf(x)
    fmt.Printf("Value: %v, Can Set: %v\n", v.Int(), v.CanSet())
}
```

## Example: Working with Structs
```go
package main
import (
    "fmt"
    "reflect"
)

type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

func main() {
    p := Person{"Alice", 30}
    t := reflect.TypeOf(p)
    
    // Inspect struct fields
    for i := 0; i < t.NumField(); i++ {
        field := t.Field(i)
        fmt.Printf("Field: %s, Type: %v, Tag: %v\n",
            field.Name, field.Type, field.Tag.Get("json"))
    }
    
    // Modify struct field
    v := reflect.ValueOf(&p).Elem()
    nameField := v.FieldByName("Name")
    if nameField.CanSet() {
        nameField.SetString("Bob")
    }
    fmt.Println("Modified struct:", p)
}
```

## Example: Creating New Values
```go
package main
import (
    "fmt"
    "reflect"
)

func main() {
    // Create new int pointer
    intType := reflect.TypeOf(0)
    intPtr := reflect.New(intType)
    intPtr.Elem().SetInt(42)
    realIntPtr := intPtr.Interface().(*int)
    fmt.Println("Value:", *realIntPtr)
    
    // Create and populate slice
    sliceType := reflect.SliceOf(intType)
    slice := reflect.MakeSlice(sliceType, 3, 5)
    for i := 0; i < 3; i++ {
        slice.Index(i).SetInt(int64(i))
    }
    fmt.Println("Slice:", slice.Interface())
}
```

## Example: Method Calls and Interfaces
```go
package main
import (
    "fmt"
    "reflect"
)

type Greeter struct{}

func (g Greeter) Hello(name string) string {
    return "Hello, " + name
}

func main() {
    g := Greeter{}
    v := reflect.ValueOf(g)
    
    // Find and call method
    method := v.MethodByName("Hello")
    args := []reflect.Value{reflect.ValueOf("World")}
    result := method.Call(args)
    fmt.Println(result[0].String())
    
    // Check if type implements interface
    var i interface{} = g
    fmt.Println("Is empty interface:", reflect.TypeOf(i).Implements(
        reflect.TypeOf((*interface{})(nil)).Elem()))
}
```

## Example: DeepEqual and Indirect
```go
package main
import (
    "fmt"
    "reflect"
)

func main() {
    // DeepEqual comparison
    slice1 := []int{1, 2, 3}
    slice2 := []int{1, 2, 3}
    fmt.Println("Slices equal:", reflect.DeepEqual(slice1, slice2))
    
    // Using Indirect to handle pointers
    x := 42
    pointer := reflect.ValueOf(&x)
    value := reflect.Indirect(pointer)
    fmt.Println("Dereferenced value:", value.Int())
}
```

See the [official documentation](https://pkg.go.dev/reflect) for more details.

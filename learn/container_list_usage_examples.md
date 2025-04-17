# Go container/list Package: Common Functions and Examples

The `container/list` package provides a doubly linked list implementation.

## Main Functions and Types
- `list.New() *list.List`: Creates a new doubly linked list.
- `(*list.List).PushBack(v interface{}) *list.Element`: Adds an element to the back.
- `(*list.List).PushFront(v interface{}) *list.Element`: Adds an element to the front.
- `(*list.List).Remove(e *list.Element) interface{}`: Removes an element.
- `(*list.List).InsertBefore(v interface{}, mark *list.Element) *list.Element`: Inserts before a mark.
- `(*list.List).InsertAfter(v interface{}, mark *list.Element) *list.Element`: Inserts after a mark.
- `(*list.List).Front() *list.Element`: Returns the first element.
- `(*list.List).Back() *list.Element`: Returns the last element.
- `type list.Element`: Represents an element in the list.
- `type list.List`: The list type itself.

## Example: Using a List
```go
package main
import (
    "container/list"
    "fmt"
)

func main() {
    l := list.New()
    l.PushBack(1)
    l.PushBack(2)
    l.PushFront(0)
    for e := l.Front(); e != nil; e = e.Next() {
        fmt.Println(e.Value)
    }
}
```

## Example: InsertBefore, InsertAfter, Remove, Front, Back
```go
package main
import (
    "container/list"
    "fmt"
)

func main() {
    l := list.New()
    e1 := l.PushBack("b")
    e2 := l.PushFront("a")
    l.InsertAfter("c", e1) // a b c
    l.InsertBefore("start", e2) // start a b c
    l.Remove(e1) // start a c
    fmt.Println("Front:", l.Front().Value) // Output: Front: start
    fmt.Println("Back:", l.Back().Value)   // Output: Back: c
    for e := l.Front(); e != nil; e = e.Next() {
        fmt.Print(e.Value, " ")
    }
    // Output: start a c
}
```

See the [official documentation](https://pkg.go.dev/container/list) for more details.

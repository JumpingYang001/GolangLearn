# Go container/heap Package: Common Functions and Examples

The `container/heap` package provides heap operations for any type that implements heap.Interface.

## Main Functions and Types
- `heap.Init(h heap.Interface)`: Initializes a heap.
- `heap.Push(h heap.Interface, x interface{})`: Pushes an element onto the heap.
- `heap.Pop(h heap.Interface) interface{}`: Removes and returns the minimum element.
- `heap.Remove(h heap.Interface, i int) interface{}`: Removes and returns the element at index i.
- `heap.Fix(h heap.Interface, i int)`: Re-establishes the heap ordering after the value at index i has changed.
- `type heap.Interface`: Interface that types must implement to use the heap functions.

## Example: Using a Min-Heap
```go
package main
import (
    "container/heap"
    "fmt"
)

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func main() {
    h := &IntHeap{2, 1, 5}
    heap.Init(h)
    heap.Push(h, 3)
    fmt.Printf("min: %d\n", (*h)[0])
    for h.Len() > 0 {
        fmt.Printf("%d ", heap.Pop(h))
    }
}
```

## Example: Remove and Fix
```go
package main
import (
    "container/heap"
    "fmt"
)

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func main() {
    h := &IntHeap{2, 1, 5, 4}
    heap.Init(h)
    heap.Remove(h, 2) // Remove element at index 2
    (*h)[0] = 10      // Change value and fix
    heap.Fix(h, 0)
    for h.Len() > 0 {
        fmt.Printf("%d ", heap.Pop(h))
    }
    // Output: 1 4 10
}
```

See the [official documentation](https://pkg.go.dev/container/heap) for more details.

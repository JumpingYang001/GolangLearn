# Go sort Package: Common Functions and Examples

The `sort` package provides primitives for sorting slices and user-defined collections.

## Main Functions and Types
- `sort.Ints(a []int)`: Sorts a slice of ints in increasing order.
- `sort.Strings(a []string)`: Sorts a slice of strings in increasing order.
- `sort.Float64s(a []float64)`: Sorts a slice of float64s in increasing order.
- `sort.Slice(slice interface{}, less func(i, j int) bool)`: Sorts a slice using a custom less function.
- `sort.Sort(data sort.Interface)`: Sorts data implementing sort.Interface.
- `sort.Search(n int, f func(int) bool) int`: Searches for the smallest index i in [0, n) at which f(i) is true.
- `type sort.Interface`: Interface for custom sorting.

## Basic Sorting Examples
```go
package main
import (
    "fmt"
    "sort"
)

func main() {
    // Sorting integers
    nums := []int{5, 2, 6, 3, 1}
    sort.Ints(nums)
    fmt.Println("Sorted ints:", nums)
    
    // Sorting strings
    names := []string{"charlie", "alice", "bob"}
    sort.Strings(names)
    fmt.Println("Sorted strings:", names)
    
    // Sorting float64s
    scores := []float64{3.14, 1.41, 2.71}
    sort.Float64s(scores)
    fmt.Println("Sorted float64s:", scores)
}
```

## Custom Sorting with sort.Slice
```go
package main
import (
    "fmt"
    "sort"
)

func main() {
    // Sort structs by field
    type Person struct {
        Name string
        Age  int
    }
    
    people := []Person{
        {"Alice", 25},
        {"Bob", 30},
        {"Charlie", 20},
    }
    
    // Sort by age
    sort.Slice(people, func(i, j int) bool {
        return people[i].Age < people[j].Age
    })
    fmt.Println("Sorted by age:", people)
    
    // Sort by name
    sort.Slice(people, func(i, j int) bool {
        return people[i].Name < people[j].Name
    })
    fmt.Println("Sorted by name:", people)
    
    // Stable sort preserves order of equal elements
    sort.SliceStable(people, func(i, j int) bool {
        return people[i].Age < people[j].Age
    })
}
```

## Implementing sort.Interface
```go
package main
import (
    "fmt"
    "sort"
)

// Custom type implementing sort.Interface
type ByLength []string

func (s ByLength) Len() int           { return len(s) }
func (s ByLength) Swap(i, j int)      { s[i], s[j] = s[j], s[i] }
func (s ByLength) Less(i, j int) bool { return len(s[i]) < len(s[j]) }

func main() {
    fruits := []string{"apple", "banana", "kiwi", "pear"}
    sort.Sort(ByLength(fruits))
    fmt.Println("Sorted by length:", fruits)
    
    // Sort in reverse
    sort.Sort(sort.Reverse(ByLength(fruits)))
    fmt.Println("Reverse sorted by length:", fruits)
}
```

## Binary Search Examples
```go
package main
import (
    "fmt"
    "sort"
)

func main() {
    // Search in sorted integers
    nums := []int{1, 2, 3, 4, 5, 6, 7, 8}
    target := 6
    
    i := sort.SearchInts(nums, target)
    if i < len(nums) && nums[i] == target {
        fmt.Printf("Found %d at index %d\n", target, i)
    }
    
    // Custom search with sort.Search
    x := 6.5
    floats := []float64{1.2, 2.3, 4.5, 6.7, 8.9}
    i = sort.Search(len(floats), func(i int) bool {
        return floats[i] >= x
    })
    if i < len(floats) {
        fmt.Printf("First value >= %.1f is at index %d (value: %.1f)\n",
            x, i, floats[i])
    }
}
```

## Advanced Sorting Examples
```go
package main
import (
    "fmt"
    "sort"
)

// Multi-field sorting
type Record struct {
    Priority int
    Name     string
    Time     int64
}

// Custom type implementing sort.Interface for multi-field sorting
type Records []Record

func (r Records) Len() int      { return len(r) }
func (r Records) Swap(i, j int) { r[i], r[j] = r[j], r[i] }
func (r Records) Less(i, j int) bool {
    // Sort by priority first
    if r[i].Priority != r[j].Priority {
        return r[i].Priority < r[j].Priority
    }
    // Then by name
    if r[i].Name != r[j].Name {
        return r[i].Name < r[j].Name
    }
    // Finally by time
    return r[i].Time < r[j].Time
}

func main() {
    records := Records{
        {2, "B", 1000},
        {1, "A", 2000},
        {1, "A", 1000},
        {2, "A", 3000},
    }
    
    // Use stable sort to maintain relative order of equal elements
    sort.Stable(records)
    fmt.Println("Multi-field sorted records:", records)
    
    // Check if slice is sorted
    fmt.Println("Is sorted?", sort.IsSorted(records))
}
```

See the [official documentation](https://pkg.go.dev/sort) for more details.

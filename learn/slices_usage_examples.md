# Go slices Package: Common Functions and Examples

The `slices` package (added in Go 1.21) provides generic slice operations.

## Main Functions and Types
- `slices.Clone(s []T) []T`: Returns a copy of the slice.
- `slices.Equal(s1, s2 []T) bool`: Reports whether two slices are equal.
- `slices.Sort(s []T)`: Sorts a slice in place.
- `slices.BinarySearch(s []T, x T) (int, bool)`: Searches for x in a sorted slice.
- `slices.Index(s []T, v T) int`: Returns the first index of v in s.
- `slices.Delete(s []T, i, j int) []T`: Deletes a range from a slice.
- `slices.Insert(s []T, i int, v ...T) []T`: Inserts values into a slice.

## Basic Operations

### Finding and Comparing Elements
```go
package main
import (
    "fmt"
    "slices"
)

func main() {
    // Check if slice contains a value
    nums := []int{1, 2, 3, 4}
    fmt.Println(slices.Contains(nums, 3)) // true
    
    // Find index of value
    names := []string{"alice", "bob", "carol"}
    fmt.Println(slices.Index(names, "bob")) // 1
    fmt.Println(slices.Index(names, "dave")) // -1
    
    // Compare slices
    s1 := []int{1, 2, 3}
    s2 := []int{1, 2, 3}
    s3 := []int{1, 2, 4}
    fmt.Println(slices.Equal(s1, s2))     // true
    fmt.Println(slices.Compare(s1, s3))   // -1 (s1 < s3)
}
```

### Modifying Slices
```go
package main
import (
    "fmt"
    "slices"
)

func main() {
    // Create a copy
    original := []int{1, 2, 3}
    copy := slices.Clone(original)
    
    // Insert elements
    nums := []int{1, 2, 3}
    nums = slices.Insert(nums, 1, 4, 5)    // Insert 4,5 at index 1
    fmt.Println(nums)                      // [1 4 5 2 3]
    
    // Delete range of elements
    nums = slices.Delete(nums, 1, 3)       // Delete elements at index 1,2
    fmt.Println(nums)                      // [1 2 3]
    
    // Replace elements
    nums = slices.Replace(nums, 1, 2, 4, 5) // Replace elements 1-2 with 4,5
    fmt.Println(nums)                       // [1 4 5 3]
}
```

### Sorting and Searching
```go
package main
import (
    "fmt"
    "slices"
)

func main() {
    // Sort slice
    nums := []int{3, 1, 4, 1, 5, 9, 2, 6}
    slices.Sort(nums)
    fmt.Println(nums)              // [1 1 2 3 4 5 6 9]
    
    // Check if sorted
    fmt.Println(slices.IsSorted(nums)) // true
    
    // Binary search (only works on sorted slices)
    i, found := slices.BinarySearch(nums, 4)
    fmt.Printf("Found 4 at index %d: %v\n", i, found)
    
    // Sort with custom comparison
    type Person struct {
        Name string
        Age  int
    }
    people := []Person{
        {"Alice", 25},
        {"Bob", 30},
        {"Carol", 20},
    }
    slices.SortFunc(people, func(a, b Person) int {
        return a.Age - b.Age
    })
    fmt.Println(people) // Sorted by age
}
```

### Advanced Operations
```go
package main
import (
    "fmt"
    "slices"
)

func main() {
    // Compact removes adjacent duplicates
    nums := []int{1, 1, 2, 2, 2, 3, 3, 1, 1}
    nums = slices.Compact(nums)
    fmt.Println(nums) // [1 2 3 1]
    
    // Grow increases capacity
    s := make([]int, 0, 2)
    s = slices.Grow(s, 10)
    fmt.Printf("Len: %d, Cap: %d\n", len(s), cap(s))
    
    // Clip reduces capacity to length
    s = append(s, 1, 2, 3)
    s = slices.Clip(s)
    fmt.Printf("After clip - Len: %d, Cap: %d\n", len(s), cap(s))
    
    // Reverse slice in place
    nums = []int{1, 2, 3, 4, 5}
    slices.Reverse(nums)
    fmt.Println(nums) // [5 4 3 2 1]
}
```

### Working with Generic Types
```go
package main
import (
    "fmt"
    "slices"
)

// Generic function that works with any comparable type
func FindAndRemove[T comparable](s []T, v T) []T {
    if i := slices.Index(s, v); i != -1 {
        return slices.Delete(s, i, i+1)
    }
    return s
}

func main() {
    // Works with any type
    ints := []int{1, 2, 3, 4, 5}
    ints = FindAndRemove(ints, 3)
    fmt.Println(ints) // [1 2 4 5]
    
    strings := []string{"a", "b", "c"}
    strings = FindAndRemove(strings, "b")
    fmt.Println(strings) // [a c]
}
```

See the [official documentation](https://pkg.go.dev/slices) for more details.

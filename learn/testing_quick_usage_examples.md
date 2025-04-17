# Go testing/quick Package: Common Functions and Examples

The `testing/quick` package implements utility functions for testing your code using random, generated input data. It's particularly useful for property-based testing and fuzz testing.

## Main Functions and Types
- `quick.Check(f interface{}, config *quick.Config) error`: Tests that a function works correctly for random inputs
- `quick.CheckEqual(f, g interface{}, config *quick.Config) error`: Tests that two functions return the same values for random inputs
- `quick.Value(t reflect.Type, rand *rand.Rand) (reflect.Value, bool)`: Generates random values of the given type
- `type quick.Config`: Configuration for running tests, including MaxCount and Rand
- `quick.Generate(value interface{}, rand *rand.Rand)`: Fills value with random data

## Basic Property Testing Example
```go
package quicktest

import (
    "testing"
    "testing/quick"
)

// Property: Reversing a string twice should return the original string
func TestStringReverseProperty(t *testing.T) {
    f := func(s string) bool {
        // First reverse
        reversed := reverseString(s)
        // Second reverse
        doubleReversed := reverseString(reversed)
        // Should equal original
        return s == doubleReversed
    }

    if err := quick.Check(f, nil); err != nil {
        t.Error(err)
    }
}

// Helper function to reverse a string
func reverseString(s string) string {
    runes := []rune(s)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    return string(runes)
}
```

## Custom Generator Example
```go
package quicktest

import (
    "testing"
    "testing/quick"
)

// Custom type
type Point struct {
    X, Y int
}

// Generate implements quick.Generator
func (Point) Generate(rand *quick.Rand, size int) reflect.Value {
    p := Point{
        X: rand.Intn(size),
        Y: rand.Intn(size),
    }
    return reflect.ValueOf(p)
}

// Property: Point distance from origin should be non-negative
func TestPointDistanceProperty(t *testing.T) {
    f := func(p Point) bool {
        distance := p.Distance()
        return distance >= 0
    }

    config := &quick.Config{
        MaxCount: 1000,  // Number of test cases to run
    }

    if err := quick.Check(f, config); err != nil {
        t.Error(err)
    }
}

func (p Point) Distance() float64 {
    return math.Sqrt(float64(p.X*p.X + p.Y*p.Y))
}
```

## Testing Function Equivalence Example
```go
package quicktest

import (
    "testing"
    "testing/quick"
)

// Testing that two implementations of factorial are equivalent
func TestFactorialEquivalence(t *testing.T) {
    // Recursive implementation
    factorial1 := func(n uint) uint {
        if n <= 1 {
            return 1
        }
        return n * factorial1(n-1)
    }

    // Iterative implementation
    factorial2 := func(n uint) uint {
        result := uint(1)
        for i := uint(2); i <= n; i++ {
            result *= i
        }
        return result
    }

    // Config to limit the size of inputs
    config := &quick.Config{
        MaxCount: 100,
        Values: func(values []reflect.Value, rand *rand.Rand) {
            values[0] = reflect.ValueOf(uint(rand.Intn(10))) // Test with small numbers
        },
    }

    if err := quick.CheckEqual(factorial1, factorial2, config); err != nil {
        t.Error(err)
    }
}
```

## Custom Configuration Example
```go
package quicktest

import (
    "testing"
    "testing/quick"
    "math/rand"
    "time"
)

func TestWithCustomConfig(t *testing.T) {
    // Property: a + b = b + a (commutative property)
    f := func(a, b int) bool {
        return a + b == b + a
    }

    // Custom configuration
    config := &quick.Config{
        MaxCount: 1000,                              // Run 1000 test cases
        Rand: rand.New(rand.NewSource(time.Now().UnixNano())), // Custom random source
        Values: func(values []reflect.Value, r *rand.Rand) {
            // Custom value generation
            values[0] = reflect.ValueOf(r.Intn(100))  // First parameter
            values[1] = reflect.ValueOf(r.Intn(100))  // Second parameter
        },
    }

    if err := quick.Check(f, config); err != nil {
        t.Error(err)
    }
}
```

## Value Generation Example
```go
package quicktest

import (
    "testing"
    "testing/quick"
    "reflect"
)

func TestValueGeneration(t *testing.T) {
    // Generate random values of different types
    types := []reflect.Type{
        reflect.TypeOf(0),          // int
        reflect.TypeOf(""),         // string
        reflect.TypeOf(true),       // bool
        reflect.TypeOf([]int{}),    // slice
        reflect.TypeOf(map[string]int{}), // map
    }

    for _, typ := range types {
        value, ok := quick.Value(typ, rand.New(rand.NewSource(time.Now().UnixNano())))
        if !ok {
            t.Errorf("Failed to generate value for type %v", typ)
            continue
        }
        t.Logf("Generated %v for type %v", value.Interface(), typ)
    }
}
```

## List Properties Testing Example
```go
package quicktest

import (
    "testing"
    "testing/quick"
    "sort"
)

func TestListProperties(t *testing.T) {
    // Property: length of sorted list equals length of original list
    lengthProperty := func(list []int) bool {
        sorted := make([]int, len(list))
        copy(sorted, list)
        sort.Ints(sorted)
        return len(sorted) == len(list)
    }

    // Property: sorted list is ordered
    sortedProperty := func(list []int) bool {
        sorted := make([]int, len(list))
        copy(sorted, list)
        sort.Ints(sorted)
        
        for i := 1; i < len(sorted); i++ {
            if sorted[i] < sorted[i-1] {
                return false
            }
        }
        return true
    }

    config := &quick.Config{
        MaxCount: 100,
        MaxCountScale: 0.1, // Reduce the number of test cases for large types
    }

    if err := quick.Check(lengthProperty, config); err != nil {
        t.Error("Length property failed:", err)
    }

    if err := quick.Check(sortedProperty, config); err != nil {
        t.Error("Sorted property failed:", err)
    }
}
```

## Important Notes
- The `testing/quick` package is most useful for property-based testing, where you test that certain properties hold true for all inputs
- Use `quick.Config` to customize:
  - The number of test cases (`MaxCount`)
  - The random number generator (`Rand`)
  - Custom value generation (`Values`)
- Always consider edge cases when defining properties
- Custom generators can be implemented using the `Generator` interface
- The package automatically generates random values for basic types and compounds of basic types
- Test functions should be deterministic and side-effect free

See the [official documentation](https://pkg.go.dev/testing/quick) for more details.

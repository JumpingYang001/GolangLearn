# Go strings Package: Common Functions and Examples

The `strings` package provides functions for manipulating UTF-8 encoded strings.

## Main Functions and Types
- `strings.Contains(s, substr string) bool`: Checks if substr is within s.
- `strings.HasPrefix(s, prefix string) bool`: Checks if s starts with prefix.
- `strings.HasSuffix(s, suffix string) bool`: Checks if s ends with suffix.
- `strings.Index(s, substr string) int`: Returns the index of substr in s.
- `strings.Split(s, sep string) []string`: Splits s into substrings.
- `strings.Join(a []string, sep string) string`: Joins a slice of strings.
- `strings.Replace(s, old, new string, n int) string`: Replaces substrings.
- `strings.ToLower(s string) string`: Converts to lowercase.
- `strings.ToUpper(s string) string`: Converts to uppercase.
- `strings.Trim(s, cutset string) string`: Trims cutset from both ends.

## Example: String Manipulation
### 1. Contains, HasPrefix, HasSuffix
Check for substrings or string prefixes/suffixes.
```go
import (
    "fmt"
    "strings"
)

fmt.Println(strings.Contains("seafood", "foo"))      // true
fmt.Println(strings.HasPrefix("Gopher", "Go"))      // true
fmt.Println(strings.HasSuffix("Amigo", "go"))       // true
```

### 2. ToUpper, ToLower, Title
Change the case of strings.
```go
fmt.Println(strings.ToUpper("hello")) // HELLO
fmt.Println(strings.ToLower("WORLD")) // world
```

### 3. Split, Join
Split a string into a slice or join a slice into a string.
```go
s := "a,b,c"
parts := strings.Split(s, ",")
fmt.Println(parts) // [a b c]
joined := strings.Join(parts, "-")
fmt.Println(joined) // a-b-c
```

### 4. Replace, ReplaceAll
Replace substrings.
```go
s := "foo bar foo"
fmt.Println(strings.Replace(s, "foo", "baz", 1))    // baz bar foo
fmt.Println(strings.ReplaceAll(s, "foo", "baz"))   // baz bar baz
```

### 5. Trim, TrimSpace
Remove unwanted characters or whitespace.
```go
fmt.Println(strings.Trim(" !! Hello!! ", " !")) // Hello
fmt.Println(strings.TrimSpace("  	Hello
"))    // Hello
```

### 6. Fields
Split a string around whitespace.
```go
s := "  foo bar   baz  "
fields := strings.Fields(s)
fmt.Println(fields) // [foo bar baz]
```

See the [official documentation](https://pkg.go.dev/strings) for more details.

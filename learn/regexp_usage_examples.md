# Go regexp Package: Common Functions and Examples

The `regexp` package provides regular expression search and pattern matching.

## Main Functions and Types
- `regexp.Compile(expr string) (*regexp.Regexp, error)`: Compiles a regular expression.
- `regexp.MustCompile(expr string) *regexp.Regexp`: Compiles a regular expression and panics on error.
- `(*regexp.Regexp).MatchString(s string) bool`: Reports whether the string matches the pattern.
- `(*regexp.Regexp).FindString(s string) string`: Returns the first match.
- `(*regexp.Regexp).FindAllString(s string, n int) []string`: Returns all matches.
- `(*regexp.Regexp).ReplaceAllString(src, repl string) string`: Replaces matches with a replacement string.
- `(*regexp.Regexp).Split(s string, n int) []string`: Splits a string by the expression.

## Example: Basic Pattern Matching
```go
package main
import (
    "fmt"
    "regexp"
)

func main() {
    // Simple pattern matching
    re := regexp.MustCompile(`\w+`)
    text := "Hello, 世界!"
    
    fmt.Println("MatchString:", re.MatchString(text))
    fmt.Println("FindString:", re.FindString(text))
    fmt.Println("FindAllString:", re.FindAllString(text, -1))
}
```

## Example: Groups and Captures
```go
package main
import (
    "fmt"
    "regexp"
)

func main() {
    // Using groups and named captures
    re := regexp.MustCompile(`(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})`)
    text := "Date: 2025-04-17"
    
    // Get all submatches
    match := re.FindStringSubmatch(text)
    if match != nil {
        names := re.SubexpNames()
        for i, name := range names {
            if i > 0 && name != "" { // Skip whole match and unnamed groups
                fmt.Printf("%s: %s\n", name, match[i])
            }
        }
    }
}
```

## Example: String Manipulation
```go
package main
import (
    "fmt"
    "regexp"
)

func main() {
    // Replace with callback function
    re := regexp.MustCompile(`(\d+)`)
    text := "Score: 123"
    result := re.ReplaceAllStringFunc(text, func(s string) string {
        return "**" + s + "**"
    })
    fmt.Println("Replace with func:", result)
    
    // Split string
    re = regexp.MustCompile(`\s*,\s*`)
    text = "apple,  banana,cherry,  dates"
    parts := re.Split(text, -1)
    fmt.Println("Split result:", parts)
}
```

## Example: Advanced Features
```go
package main
import (
    "fmt"
    "regexp"
)

func main() {
    // Case-insensitive matching
    re := regexp.MustCompile(`(?i)hello`)
    fmt.Println("Case-insensitive:", re.MatchString("HELLO"))
    
    // Word boundaries
    re = regexp.MustCompile(`\bword\b`)
    fmt.Println("Word boundary:", re.MatchString("a word here"))
    
    // Multiline mode
    re = regexp.MustCompile(`(?m)^line`)
    text := "first line\nsecond line"
    fmt.Println("Multiline matches:", re.FindAllString(text, -1))
    
    // Look-around (Go doesn't support lookbehind)
    re = regexp.MustCompile(`\w+(?=\s*\d)`)
    fmt.Println("Lookahead:", re.FindString("Version 123"))
}
```

See the [official documentation](https://pkg.go.dev/regexp) for more details.

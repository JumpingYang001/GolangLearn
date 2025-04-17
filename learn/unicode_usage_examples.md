# Go unicode Package: Common Functions and Examples

The `unicode` package provides data and functions to test various properties of Unicode characters. It's essential for handling Unicode text and character classifications.

## Main Functions and Types
- `unicode.IsLetter(r rune)`: Tests if a rune is a letter
- `unicode.IsDigit(r rune)`: Tests if a rune is a decimal digit
- `unicode.IsNumber(r rune)`: Tests if a rune is a number (includes digits and other number-like characters)
- `unicode.IsSpace(r rune)`: Tests if a rune is a whitespace character
- `unicode.IsPunct(r rune)`: Tests if a rune is a punctuation character
- `unicode.IsSymbol(r rune)`: Tests if a rune is a symbol character
- `unicode.IsControl(r rune)`: Tests if a rune is a control character
- `unicode.IsGraphic(r rune)`: Tests if a rune is graphic (printable)

## Basic Character Classification Example
```go
package main

import (
    "fmt"
    "unicode"
)

func main() {
    // Test different types of characters
    chars := []rune{
        'A', '1', 'π', '世', ' ', '!', '\n', '★',
    }

    for _, ch := range chars {
        fmt.Printf("Character: %c (U+%04X)\n", ch, ch)
        fmt.Printf("  IsLetter: %t\n", unicode.IsLetter(ch))
        fmt.Printf("  IsDigit:  %t\n", unicode.IsDigit(ch))
        fmt.Printf("  IsSpace:  %t\n", unicode.IsSpace(ch))
        fmt.Printf("  IsPunct:  %t\n", unicode.IsPunct(ch))
        fmt.Printf("  IsSymbol: %t\n", unicode.IsSymbol(ch))
        fmt.Printf("  IsControl: %t\n", unicode.IsControl(ch))
        fmt.Println()
    }
}
```

## Unicode Ranges and Categories Example
```go
package main

import (
    "fmt"
    "unicode"
)

func main() {
    // Test different Unicode ranges
    ranges := []struct {
        name string
        r    rune
    }{
        {"Latin", 'A'},
        {"Greek", 'π'},
        {"Cyrillic", 'Ж'},
        {"Chinese", '世'},
        {"Japanese", 'あ'},
        {"Korean", '한'},
        {"Arabic", 'ا'},
        {"Emoji", '😀'},
    }

    for _, test := range ranges {
        r := test.r
        fmt.Printf("Character %c from %s:\n", r, test.name)
        fmt.Printf("  In Han: %t\n", unicode.Is(unicode.Han, r))
        fmt.Printf("  In Hiragana: %t\n", unicode.Is(unicode.Hiragana, r))
        fmt.Printf("  In Greek: %t\n", unicode.Is(unicode.Greek, r))
        fmt.Printf("  Upper: %t\n", unicode.IsUpper(r))
        fmt.Printf("  Lower: %t\n", unicode.IsLower(r))
        fmt.Printf("  Title: %t\n", unicode.IsTitle(r))
        fmt.Println()
    }
}
```

## Case Folding and Conversion Example
```go
package main

import (
    "fmt"
    "unicode"
)

func main() {
    // Test case conversions
    chars := []rune{
        'A', 'a', 'Σ', 'σ', 'ς', 'Ǆ',
    }

    for _, ch := range chars {
        fmt.Printf("Original: %c (U+%04X)\n", ch, ch)
        fmt.Printf("  ToUpper: %c (U+%04X)\n", 
            unicode.ToUpper(ch), unicode.ToUpper(ch))
        fmt.Printf("  ToLower: %c (U+%04X)\n", 
            unicode.ToLower(ch), unicode.ToLower(ch))
        fmt.Printf("  ToTitle: %c (U+%04X)\n", 
            unicode.ToTitle(ch), unicode.ToTitle(ch))
        fmt.Println()
    }
}
```

## Script Detection Example
```go
package main

import (
    "fmt"
    "unicode"
)

func main() {
    // Detect scripts of different characters
    testCases := []struct {
        char rune
        name string
    }{
        {'A', "Latin A"},
        {'α', "Greek alpha"},
        {'א', "Hebrew alef"},
        {'ا', "Arabic alif"},
        {'क', "Devanagari ka"},
        {'よ', "Hiragana yo"},
        {'한', "Hangul han"},
        {'漢', "Han (kanji)"},
    }

    for _, tc := range testCases {
        fmt.Printf("Character: %c (%s)\n", tc.char, tc.name)
        
        // Test various scripts
        fmt.Printf("  Latin: %t\n", unicode.Is(unicode.Latin, tc.char))
        fmt.Printf("  Greek: %t\n", unicode.Is(unicode.Greek, tc.char))
        fmt.Printf("  Arabic: %t\n", unicode.Is(unicode.Arabic, tc.char))
        fmt.Printf("  Hebrew: %t\n", unicode.Is(unicode.Hebrew, tc.char))
        fmt.Printf("  Devanagari: %t\n", unicode.Is(unicode.Devanagari, tc.char))
        fmt.Printf("  Han: %t\n", unicode.Is(unicode.Han, tc.char))
        fmt.Printf("  Hiragana: %t\n", unicode.Is(unicode.Hiragana, tc.char))
        fmt.Printf("  Hangul: %t\n", unicode.Is(unicode.Hangul, tc.char))
        fmt.Println()
    }
}
```

## Important Notes
- The unicode package provides character classification based on Unicode Standard categories
- Unicode properties are based on the Unicode Character Database
- Key character categories include:
  - Letters (L): uppercase, lowercase, titlecase, modifier, other
  - Numbers (N): decimal digit, letter numbers, other numbers
  - Punctuation (P): dash, open, close, initial quote, final quote, other
  - Symbols (S): math, currency, modifier, other
  - Marks (M): non-spacing, spacing combining, enclosing
  - Separators (Z): space, line, paragraph
  - Control (C): control, format, surrogate, private-use
- The package provides predefined ranges for various scripts (Latin, Greek, Cyrillic, etc.)
- Case conversion functions handle special cases like:
  - Final sigma in Greek
  - Title case differences
  - Locale-independent mappings
- The package is used internally by many other packages for Unicode support

See the [official documentation](https://pkg.go.dev/unicode) for more details.

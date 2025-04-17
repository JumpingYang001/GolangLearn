# Go unicode/utf8 Package: Common Functions and Examples

The `unicode/utf8` package implements functions and constants to support text encoded in UTF-8, which is the default encoding for text in Go.

## Main Functions and Types
- `utf8.RuneLen(r rune)`: Returns the number of bytes needed to encode the rune
- `utf8.EncodeRune(p []byte, r rune)`: Encodes a rune into bytes
- `utf8.DecodeRune(p []byte)`: Decodes a rune from bytes
- `utf8.RuneCount(p []byte)`: Counts the number of runes in UTF-8-encoded bytes
- `utf8.Valid(p []byte)`: Reports whether p is a valid UTF-8 encoding
- `utf8.ValidRune(r rune)`: Reports whether r can be encoded as UTF-8
- `utf8.ValidString(s string)`: Reports whether s is valid UTF-8

## Basic UTF-8 Operations Example
```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    // Sample text with various Unicode characters
    text := "Hello, 世界! 🌍"
    
    // Count runes (characters)
    runeCount := utf8.RuneCountInString(text)
    byteCount := len(text)
    fmt.Printf("String: %s\n", text)
    fmt.Printf("Bytes: %d, Runes: %d\n", byteCount, runeCount)
    
    // Iterate and analyze each rune
    for i, r := range text {
        size := utf8.RuneLen(r)
        fmt.Printf("Rune: %c, Position: %d, Size: %d bytes\n", 
            r, i, size)
    }
}
```

## UTF-8 Encoding/Decoding Example
```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    // Example runes to encode
    runes := []rune{
        'A',    // Basic ASCII
        '世',    // CJK character
        '🌍',    // Emoji (outside BMP)
    }
    
    for _, r := range runes {
        // Get required buffer size
        size := utf8.RuneLen(r)
        buf := make([]byte, size)
        
        // Encode rune
        utf8.EncodeRune(buf, r)
        
        fmt.Printf("Rune: %c (U+%04X)\n", r, r)
        fmt.Printf("UTF-8 bytes: % X\n", buf)
        
        // Decode back
        decoded, _ := utf8.DecodeRune(buf)
        fmt.Printf("Decoded: %c\n\n", decoded)
    }
}
```

## UTF-8 Validation Example
```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func analyzeBytes(b []byte) {
    fmt.Printf("Input bytes: % X\n", b)
    fmt.Printf("Valid UTF-8: %v\n", utf8.Valid(b))
    
    if !utf8.Valid(b) {
        // Try to find invalid sequence
        for i := 0; i < len(b); {
            r, size := utf8.DecodeRune(b[i:])
            if r == utf8.RuneError && size == 1 {
                fmt.Printf("Invalid byte at position %d: %X\n", i, b[i])
            }
            i += size
        }
    }
}

func main() {
    // Valid UTF-8 sequences
    valid := []byte("Hello, 世界")
    fmt.Println("Testing valid UTF-8:")
    analyzeBytes(valid)
    
    // Invalid UTF-8 sequences
    invalid := []byte{0x48, 0x65, 0xFF, 0x6C, 0x6C, 0x6F}
    fmt.Println("\nTesting invalid UTF-8:")
    analyzeBytes(invalid)
}
```

## UTF-8 String Processing Example
```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func processUTF8String(s string) {
    for len(s) > 0 {
        r, size := utf8.DecodeRuneInString(s)
        if r == utf8.RuneError {
            fmt.Println("Invalid UTF-8 sequence found")
            break
        }
        
        fmt.Printf("Rune: %c, Size: %d bytes, Code point: U+%04X\n",
            r, size, r)
        
        s = s[size:]
    }
}

func main() {
    // Process mixed ASCII and Unicode text
    text := "Hello, 世界! 🌍"
    fmt.Println("Processing UTF-8 string:")
    processUTF8String(text)
    
    // String length vs rune count
    fmt.Printf("\nString byte length: %d\n", len(text))
    fmt.Printf("String rune count: %d\n", 
        utf8.RuneCountInString(text))
}
```

## Working with UTF-8 Buffer Example
```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func appendRune(buf []byte, r rune) []byte {
    size := utf8.RuneLen(r)
    if size == -1 {
        // Invalid rune
        return buf
    }
    
    start := len(buf)
    if start+size > cap(buf) {
        // Need more space
        newBuf := make([]byte, start, start+size)
        copy(newBuf, buf)
        buf = newBuf
    }
    
    // Extend slice and encode rune
    buf = buf[:start+size]
    utf8.EncodeRune(buf[start:], r)
    return buf
}

func main() {
    // Create a UTF-8 buffer
    buf := make([]byte, 0, 16)
    
    // Append various runes
    runes := []rune{'H', 'i', '世', '界', '🌍'}
    for _, r := range runes {
        buf = appendRune(buf, r)
    }
    
    // Analyze result
    fmt.Printf("Buffer contents: %s\n", string(buf))
    fmt.Printf("Buffer bytes: % X\n", buf)
    fmt.Printf("Buffer length: %d bytes\n", len(buf))
    fmt.Printf("Rune count: %d\n", utf8.RuneCount(buf))
}
```

## Important Notes
- UTF-8 is the default encoding for Go source code and strings
- UTF-8 uses variable-width encoding:
  - 1 byte for ASCII characters (0-127)
  - 2-4 bytes for other Unicode code points
- Key concepts:
  - Valid UTF-8 sequences always follow specific patterns
  - First byte indicates sequence length
  - Continuation bytes start with 10 in binary
- Error handling:
  - `RuneError` (U+FFFD) is used for invalid sequences
  - Size of 1 with `RuneError` indicates invalid byte
- Performance considerations:
  - ASCII is efficient (1 byte per character)
  - String indexing is byte-based, not rune-based
  - Use `range` for rune-by-rune iteration
- Common operations:
  - Count runes: `utf8.RuneCountInString()`
  - Validate UTF-8: `utf8.Valid()` or `utf8.ValidString()`
  - Get rune length: `utf8.RuneLen()`
  - Encode/decode: `utf8.EncodeRune()` and `utf8.DecodeRune()`

See the [official documentation](https://pkg.go.dev/unicode/utf8) for more details.

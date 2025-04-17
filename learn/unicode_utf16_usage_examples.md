# Go unicode/utf16 Package: Common Functions and Examples

The `unicode/utf16` package provides encoding and decoding of UTF-16 sequences. It's particularly useful when working with systems or file formats that use UTF-16 encoding.

## Main Functions and Types
- `utf16.Encode([]rune) []uint16`: Encodes a slice of runes into UTF-16
- `utf16.Decode([]uint16) []rune`: Decodes UTF-16 into a slice of runes
- `utf16.EncodeRune(rune) (r1, r2 uint16)`: Encodes a rune as a surrogate pair
- `utf16.DecodeRune(r1, r2 uint16) rune`: Decodes a surrogate pair
- `utf16.IsSurrogate(r uint16)`: Tests if a uint16 is a surrogate
- `utf16.IsHighSurrogate(r uint16)`: Tests if a uint16 is a high surrogate
- `utf16.IsLowSurrogate(r uint16)`: Tests if a uint16 is a low surrogate

## Basic UTF-16 Encoding/Decoding Example
```go
package main

import (
    "fmt"
    "unicode/utf16"
)

func main() {
    // Sample text with BMP and non-BMP characters
    text := []rune("Hello, 世界! 🌍")
    
    // Encode to UTF-16
    utf16Encoded := utf16.Encode(text)
    fmt.Println("UTF-16 encoded values:")
    for i, v := range utf16Encoded {
        fmt.Printf("  [%d]: 0x%04X\n", i, v)
    }
    
    // Decode back from UTF-16
    decoded := utf16.Decode(utf16Encoded)
    fmt.Printf("\nDecoded text: %s\n", string(decoded))
    
    // Verify roundtrip
    fmt.Printf("Roundtrip successful: %v\n", 
        string(text) == string(decoded))
}
```

## Surrogate Pair Handling Example
```go
package main

import (
    "fmt"
    "unicode/utf16"
)

func main() {
    // Example with characters requiring surrogate pairs
    chars := []rune{
        '🌍', // Earth globe (U+1F30D)
        '𝄞', // Musical G clef (U+1D11E)
        '👋', // Waving hand (U+1F44B)
    }
    
    for _, ch := range chars {
        // Encode single rune to surrogate pair
        r1, r2 := utf16.EncodeRune(ch)
        
        fmt.Printf("Character: %c (U+%X)\n", ch, ch)
        fmt.Printf("  High surrogate: 0x%04X\n", r1)
        fmt.Printf("  Low surrogate:  0x%04X\n", r2)
        
        // Verify surrogate properties
        fmt.Printf("  IsHighSurrogate(r1): %v\n", 
            utf16.IsHighSurrogate(r1))
        fmt.Printf("  IsLowSurrogate(r2): %v\n", 
            utf16.IsLowSurrogate(r2))
        
        // Decode back to original rune
        decoded := utf16.DecodeRune(r1, r2)
        fmt.Printf("  Decoded: %c (U+%X)\n\n", 
            decoded, decoded)
    }
}
```

## UTF-16 String Processing Example
```go
package main

import (
    "fmt"
    "unicode/utf16"
)

func processUTF16String(u16 []uint16) {
    i := 0
    for i < len(u16) {
        var r rune
        
        // Check if current uint16 is part of a surrogate pair
        if i+1 < len(u16) && 
           utf16.IsHighSurrogate(u16[i]) && 
           utf16.IsLowSurrogate(u16[i+1]) {
            // Decode surrogate pair
            r = utf16.DecodeRune(u16[i], u16[i+1])
            i += 2
        } else {
            // Single UTF-16 code unit
            r = rune(u16[i])
            i++
        }
        
        fmt.Printf("Rune: %c (U+%04X)\n", r, r)
    }
}

func main() {
    // Mixed BMP and non-BMP text
    text := []rune("Hello世界🌍")
    utf16Text := utf16.Encode(text)
    
    fmt.Println("Processing UTF-16 string:")
    processUTF16String(utf16Text)
}
```

## Working with UTF-16 Byte Order Mark (BOM)
```go
package main

import (
    "fmt"
    "unicode/utf16"
)

const (
    BOM_UTF16_BE = 0xFEFF // Big-endian
    BOM_UTF16_LE = 0xFFFE // Little-endian
)

func detectAndDecodeUTF16(data []uint16) (string, string) {
    if len(data) == 0 {
        return "", "No BOM detected"
    }

    var endianness string
    var startIdx int

    // Check for BOM
    switch data[0] {
    case BOM_UTF16_BE:
        endianness = "Big-endian"
        startIdx = 1
    case BOM_UTF16_LE:
        endianness = "Little-endian"
        startIdx = 1
    default:
        endianness = "No BOM detected"
        startIdx = 0
    }

    // Decode the text after BOM
    decoded := utf16.Decode(data[startIdx:])
    return string(decoded), endianness
}

func main() {
    // Example with BE BOM
    textWithBEBOM := []uint16{0xFEFF, 0x0048, 0x0069} // "Hi" with BE BOM
    textBE, endiannessBE := detectAndDecodeUTF16(textWithBEBOM)
    fmt.Printf("BE BOM Text: %s (%s)\n", textBE, endiannessBE)

    // Example with LE BOM
    textWithLEBOM := []uint16{0xFFFE, 0x0048, 0x0069} // "Hi" with LE BOM
    textLE, endiannessLE := detectAndDecodeUTF16(textWithLEBOM)
    fmt.Printf("LE BOM Text: %s (%s)\n", textLE, endiannessLE)

    // Example without BOM
    textNoBOM := []uint16{0x0048, 0x0069} // "Hi" without BOM
    textPlain, endiannessPlain := detectAndDecodeUTF16(textNoBOM)
    fmt.Printf("No BOM Text: %s (%s)\n", textPlain, endiannessPlain)
}
```

## Important Notes
- UTF-16 uses either 2 or 4 bytes to represent a Unicode code point
- Characters in Basic Multilingual Plane (BMP, U+0000 to U+FFFF) use 2 bytes
- Characters outside BMP use 4 bytes (surrogate pairs)
- Surrogate pairs:
  - High surrogate: U+D800 to U+DBFF
  - Low surrogate: U+DC00 to U+DFFF
- The package handles surrogate pairs automatically in Encode/Decode
- Manual surrogate pair handling is needed when processing UTF-16 data directly
- Byte Order Mark (BOM) handling is not automatic
- Common UTF-16 use cases:
  - Windows API calls
  - Java String representation
  - Various file formats (XML, PDF)

See the [official documentation](https://pkg.go.dev/unicode/utf16) for more details.

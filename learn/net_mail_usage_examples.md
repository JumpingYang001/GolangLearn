# Go net/mail Package: Common Functions and Examples

The `net/mail` package provides parsing of mail messages.

## Main Functions and Types
- `mail.ReadMessage(r io.Reader) (*mail.Message, error)`: Reads a mail message from a reader.
- `mail.ParseAddress(address string) (*mail.Address, error)`: Parses a single email address.
- `mail.ParseAddressList(list string) ([]*mail.Address, error)`: Parses a list of email addresses.
- `type mail.Message`: Represents a parsed mail message.
- `type mail.Address`: Represents an email address.

## Example: Parsing Email Messages
```go
package main
import (
    "net/mail"
    "fmt"
    "strings"
)

func main() {
    msg := `From: sender@example.com
To: receiver@example.com
Subject: Hello

This is the body.`
    r := strings.NewReader(msg)
    m, err := mail.ReadMessage(r)
    if err != nil {
        panic(err)
    }
    fmt.Println("Subject:", m.Header.Get("Subject"))
}
```

## Example: Parsing Email Addresses
```go
package main
import (
    "net/mail"
    "fmt"
)

func main() {
    // Parse single address
    addr, err := mail.ParseAddress("John Doe <john@example.com>")
    if err != nil {
        panic(err)
    }
    fmt.Printf("Name: %s, Email: %s\n", addr.Name, addr.Address)

    // Parse address list
    addrs, err := mail.ParseAddressList("alice@example.com, Bob <bob@example.com>")
    if err != nil {
        panic(err)
    }
    for _, addr := range addrs {
        fmt.Printf("Name: %s, Email: %s\n", addr.Name, addr.Address)
    }
}
```

See the [official documentation](https://pkg.go.dev/net/mail) for more details.

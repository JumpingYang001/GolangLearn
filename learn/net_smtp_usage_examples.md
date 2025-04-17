# Go net/smtp Package: Common Functions and Examples

The `net/smtp` package provides functions for sending email using the Simple Mail Transfer Protocol (SMTP).

## Main Functions and Types
- `smtp.SendMail(addr string, a smtp.Auth, from string, to []string, msg []byte) error`: Sends an email.
- `smtp.PlainAuth(identity, username, password, host string) smtp.Auth`: Returns an Auth for plain authentication.
- `type smtp.Auth`: Interface for SMTP authentication.

## Example: Sending Email (localhost, no auth)
```go
package main
import (
    "net/smtp"
    "log"
)

func main() {
    err := smtp.SendMail(
        "localhost:25",
        nil,
        "from@example.com",
        []string{"to@example.com"},
        []byte("Subject: Test\r\n\r\nHello, email!"),
    )
    if err != nil {
        log.Fatal(err)
    }
}
```

## Example: Sending Email with Authentication
```go
package main
import (
    "net/smtp"
    "log"
)

func main() {
    // Configure authentication
    auth := smtp.PlainAuth(
        "",                     // identity (optional)
        "user@example.com",     // username
        "password",            // password
        "smtp.example.com",    // host
    )

    // Compose message
    msg := []byte("Subject: Authenticated Email\r\n" +
        "\r\n" +
        "This email was sent using SMTP authentication.")

    // Send email
    err := smtp.SendMail(
        "smtp.example.com:587",
        auth,
        "from@example.com",
        []string{"to@example.com"},
        msg,
    )
    if err != nil {
        log.Fatal(err)
    }
}
```

See the [official documentation](https://pkg.go.dev/net/smtp) for more details.

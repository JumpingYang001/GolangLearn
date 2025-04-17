# Go os/user Package: Common Functions and Examples

The `os/user` package provides functions for user account lookups.

## Main Functions and Types
- `user.Current() (*user.User, error)`: Returns the current user.
- `user.Lookup(username string) (*user.User, error)`: Looks up a user by username.
- `user.LookupId(uid string) (*user.User, error)`: Looks up a user by user ID.
- `user.LookupGroup(groupname string) (*user.Group, error)`: Looks up a group by name.
- `user.LookupGroupId(gid string) (*user.Group, error)`: Looks up a group by group ID.
- `type user.User`: Represents a user account.
- `type user.Group`: Represents a user group.

## Example: Current User
```go
package main
import (
    "os/user"
    "fmt"
)

func main() {
    // Get current user
    u, err := user.Current()
    if err != nil {
        panic(err)
    }
    fmt.Printf("Current user:\n")
    fmt.Printf("  Username: %s\n", u.Username)
    fmt.Printf("  Name: %s\n", u.Name)
    fmt.Printf("  HomeDir: %s\n", u.HomeDir)
    fmt.Printf("  UID: %s\n", u.Uid)
    fmt.Printf("  GID: %s\n", u.Gid)
}
```

## Example: Looking Up Users
```go
package main
import (
    "os/user"
    "fmt"
)

func main() {
    // Look up by username
    u1, err := user.Lookup("admin")
    if err != nil {
        fmt.Println("Lookup error:", err)
    } else {
        fmt.Printf("User by name: %s (uid=%s)\n", u1.Username, u1.Uid)
    }

    // Look up by user ID
    u2, err := user.LookupId("1000")
    if err != nil {
        fmt.Println("LookupId error:", err)
    } else {
        fmt.Printf("User by ID: %s (name=%s)\n", u2.Username, u2.Name)
    }
}
```

## Example: Working with Groups
```go
package main
import (
    "os/user"
    "fmt"
)

func main() {
    // Look up group by name
    g1, err := user.LookupGroup("wheel")
    if err != nil {
        fmt.Println("LookupGroup error:", err)
    } else {
        fmt.Printf("Group by name: %s (gid=%s)\n", g1.Name, g1.Gid)
    }

    // Look up group by ID
    g2, err := user.LookupGroupId("1000")
    if err != nil {
        fmt.Println("LookupGroupId error:", err)
    } else {
        fmt.Printf("Group by ID: %s\n", g2.Name)
    }

    // Get user's groups
    current, _ := user.Current()
    groups, err := current.GroupIds()
    if err != nil {
        fmt.Println("GroupIds error:", err)
    } else {
        fmt.Println("Current user's group IDs:", groups)
    }
}
```

See the [official documentation](https://pkg.go.dev/os/user) for more details.

# Go text/template Package: Common Functions and Examples

The `text/template` package implements data-driven templates for generating textual output. It's commonly used for generating documents, configuration files, and other text-based content.

## Main Functions and Types
- `template.New(name string)`: Creates a new template with the given name
- `template.Parse(text string)`: Parses template text into a template
- `template.ParseFiles(filenames ...string)`: Creates a template from files
- `template.Must(t *Template, err error)`: Helper that wraps template creation
- `template.Execute(wr io.Writer, data interface{})`: Applies template to data
- `template.ExecuteTemplate(wr io.Writer, name string, data interface{})`: Executes specific named template
- `template.Funcs(funcMap FuncMap)`: Adds functions to template's function map

## Basic Template Example
```go
package main

import (
    "os"
    "text/template"
)

func main() {
    // Simple template with variable substitution
    const tmpl = `Hello, {{.Name}}!
Age: {{.Age}}
City: {{.City}}`

    // Create and parse template
    t := template.Must(template.New("person").Parse(tmpl))

    // Data to be used in template
    data := struct {
        Name string
        Age  int
        City string
    }{
        Name: "Alice",
        Age:  30,
        City: "New York",
    }

    // Execute template
    err := t.Execute(os.Stdout, data)
    if err != nil {
        panic(err)
    }
}
```

## Template Functions Example
```go
package main

import (
    "os"
    "strings"
    "text/template"
    "time"
)

func main() {
    // Define custom functions
    funcMap := template.FuncMap{
        "title":     strings.Title,
        "upper":     strings.ToUpper,
        "formatDate": func(t time.Time) string {
            return t.Format("2006-01-02")
        },
    }

    // Template using built-in and custom functions
    const tmpl = `Name: {{.Name | title}}
Description: {{.Description | upper}}
Created: {{.CreatedAt | formatDate}}`

    // Create template with function map
    t := template.Must(template.New("doc").Funcs(funcMap).Parse(tmpl))

    // Data structure
    data := struct {
        Name        string
        Description string
        CreatedAt   time.Time
    }{
        Name:        "sample document",
        Description: "this is a test",
        CreatedAt:   time.Now(),
    }

    // Execute template
    err := t.Execute(os.Stdout, data)
    if err != nil {
        panic(err)
    }
}
```

## Control Structures Example
```go
package main

import (
    "os"
    "text/template"
)

func main() {
    const tmpl = `
{{if .Success}}
    Operation completed successfully!
    {{if .HasResults}}
        Results found: {{len .Items}}
        {{range .Items}}
        - {{.Name}}: {{.Value}}
        {{end}}
    {{else}}
        No results found.
    {{end}}
{{else}}
    Operation failed: {{.Error}}
{{end}}

{{with .User}}
    User Details:
    Name: {{.Name}}
    Email: {{.Email}}
{{end}}`

    data := struct {
        Success    bool
        HasResults bool
        Items      []struct{ Name, Value string }
        Error      string
        User       struct{ Name, Email string }
    }{
        Success:    true,
        HasResults: true,
        Items: []struct{ Name, Value string }{
            {"Item1", "Value1"},
            {"Item2", "Value2"},
        },
        User: struct{ Name, Email string }{
            Name:  "John Doe",
            Email: "john@example.com",
        },
    }

    t := template.Must(template.New("control").Parse(tmpl))
    err := t.Execute(os.Stdout, data)
    if err != nil {
        panic(err)
    }
}
```

## Template Composition Example
```go
package main

import (
    "os"
    "text/template"
)

func main() {
    // Define templates
    templates := map[string]string{
        "header": `
====================================
{{.Title}}
====================================`,
        "content": `
{{template "header" .}}

Content:
{{range .Items}}
- {{.}}
{{end}}

{{template "footer" .}}`,
        "footer": `
====================================
© {{.Year}} {{.Company}}
====================================`,
    }

    // Create template set
    tmpl := template.New("content")
    for name, text := range templates {
        template.Must(tmpl.New(name).Parse(text))
    }

    // Data for templates
    data := struct {
        Title   string
        Items   []string
        Year    int
        Company string
    }{
        Title:   "Template Composition Example",
        Items:   []string{"Item 1", "Item 2", "Item 3"},
        Year:    2025,
        Company: "Example Corp",
    }

    // Execute main template
    err := tmpl.Execute(os.Stdout, data)
    if err != nil {
        panic(err)
    }
}
```

## File Template Example
```go
package main

import (
    "os"
    "text/template"
)

// Template file content (config.tmpl):
/*
server {
    listen      {{.Port}};
    server_name {{.ServerName}};

    location / {
        root   {{.DocumentRoot}};
        index  {{range .IndexFiles}}{{.}} {{end}};
    }

    {{if .SSL}}
    ssl_certificate     {{.SSLCert}};
    ssl_certificate_key {{.SSLKey}};
    {{end}}
}
*/

func main() {
    // Parse template file
    tmpl, err := template.ParseFiles("config.tmpl")
    if err != nil {
        panic(err)
    }

    // Configuration data
    config := struct {
        Port         int
        ServerName   string
        DocumentRoot string
        IndexFiles  []string
        SSL         bool
        SSLCert     string
        SSLKey      string
    }{
        Port:         80,
        ServerName:   "example.com",
        DocumentRoot: "/var/www/html",
        IndexFiles:   []string{"index.html", "index.htm"},
        SSL:         true,
        SSLCert:     "/etc/ssl/cert.pem",
        SSLKey:      "/etc/ssl/key.pem",
    }

    // Execute template to file
    f, err := os.Create("nginx.conf")
    if err != nil {
        panic(err)
    }
    defer f.Close()

    err = tmpl.Execute(f, config)
    if err != nil {
        panic(err)
    }
}
```

## Custom Delimiters Example
```go
package main

import (
    "os"
    "text/template"
)

func main() {
    // Template with custom delimiters
    const tmpl = `
[% if .IsAdmin %]
Welcome, Admin!
Available actions:
[% range .Actions %]
  * [% . %]
[% end %]
[% else %]
Welcome, Guest!
[% end %]`

    // Create template with custom delimiters
    t := template.New("custom")
    t.Delims("[%", "%]")
    template.Must(t.Parse(tmpl))

    // Data structure
    data := struct {
        IsAdmin bool
        Actions []string
    }{
        IsAdmin: true,
        Actions: []string{"Create", "Edit", "Delete"},
    }

    // Execute template
    err := t.Execute(os.Stdout, data)
    if err != nil {
        panic(err)
    }
}
```

## Important Notes
- Template syntax uses `{{` and `}}` as default delimiters
- Common template actions:
  - `{{.}}` - Current value
  - `{{.Field}}` - Field access
  - `{{if .Condition}} {{else}} {{end}}` - Conditional
  - `{{range .Items}} {{end}}` - Iteration
  - `{{with .Value}} {{end}}` - Variable scope
  - `{{template "name" .}}` - Template inclusion
- Built-in functions include:
  - `len` - Length of array, slice, map, or string
  - `and`, `or`, `not` - Boolean operations
  - `eq`, `ne`, `lt`, `le`, `gt`, `ge` - Comparisons
  - `print`, `printf`, `println` - Formatted output
- Template functions must return either one value, or one value and an error
- Templates are executed in a secure environment
- Undefined values are replaced with empty strings
- The `html/template` package should be used instead for HTML output to prevent XSS

See the [official documentation](https://pkg.go.dev/text/template) for more details.

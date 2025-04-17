# archive/tar Package Usage Examples

The `archive/tar` package implements access to tar archive files. It supports reading and writing tar format archives.

## Main Functions and Types
- `tar.NewReader(r io.Reader)`: Creates a new tar reader
- `tar.NewWriter(w io.Writer)`: Creates a new tar writer
- `type Header`: Contains file metadata for tar entries
  - `Name`: Name of the file
  - `Mode`: Permission and mode bits
  - `Size`: Logical file size in bytes
  - `ModTime`: Modification time
  - `Typeflag`: Type of header entry
  - `Linkname`: Target name of link
- `(*Writer).WriteHeader(hdr *Header)`: Writes tar header
- `(*Writer).Write(b []byte)`: Writes file content
- `(*Reader).Next()`: Advances to next entry in tar archive
- `(*Reader).Read(b []byte)`: Reads from current file in tar
- Common TypeFlag values:
  - `TypeReg`: Regular file
  - `TypeDir`: Directory
  - `TypeSymlink`: Symbolic link
  - `TypeLink`: Hard link

## Creating a tar archive

```go
package main

import (
    "archive/tar"
    "fmt"
    "os"
)

func main() {
    // Create a new tar file
    tarFile, err := os.Create("archive.tar")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer tarFile.Close()

    // Create a new tar writer
    tw := tar.NewWriter(tarFile)
    defer tw.Close()

    // Add a file to the tar archive
    fileToAdd := []byte("Hello, this is the content of the file!")
    hdr := &tar.Header{
        Name: "example.txt",
        Mode: 0600,
        Size: int64(len(fileToAdd)),
    }

    if err := tw.WriteHeader(hdr); err != nil {
        fmt.Println(err)
        return
    }
    if _, err := tw.Write(fileToAdd); err != nil {
        fmt.Println(err)
        return
    }
}
```

## Reading from a tar archive

```go
package main

import (
    "archive/tar"
    "fmt"
    "io"
    "os"
)

func main() {
    // Open the tar file
    tarFile, err := os.Open("archive.tar")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer tarFile.Close()

    // Create a new tar reader
    tr := tar.NewReader(tarFile)

    // Iterate through the files in the archive
    for {
        header, err := tr.Next()
        if err == io.EOF {
            break // End of archive
        }
        if err != nil {
            fmt.Println(err)
            return
        }

        fmt.Printf("File: %s\n", header.Name)
        
        // Read the file content
        content := make([]byte, header.Size)
        if _, err := tr.Read(content); err != nil && err != io.EOF {
            fmt.Println(err)
            return
        }
        
        fmt.Printf("Content: %s\n", string(content))
    }
}
```

## Appending to an existing tar archive

```go
package main

import (
    "archive/tar"
    "fmt"
    "os"
)

func main() {
    // Open existing tar file for appending
    tarFile, err := os.OpenFile("archive.tar", os.O_RDWR|os.O_APPEND, 0644)
    if err != nil {
        fmt.Println(err)
        return
    }
    defer tarFile.Close()

    // Create tar writer
    tw := tar.NewWriter(tarFile)
    defer tw.Close()

    // Add new file to archive
    newContent := []byte("This is a new file in the archive!")
    hdr := &tar.Header{
        Name: "newfile.txt",
        Mode: 0600,
        Size: int64(len(newContent)),
    }

    if err := tw.WriteHeader(hdr); err != nil {
        fmt.Println(err)
        return
    }
    if _, err := tw.Write(newContent); err != nil {
        fmt.Println(err)
        return
    }
}
```


To truncate the content of a file under a certain dimension, and leave the file untouched if it's smaller, there is the `os.Truncate` method. Its usage is pretty simple, as shown in the following code:

```go
package main

import "os"

func main() {
    // let's keep thing under 4kB
    if err := os.Truncate("file.txt", 4096); err != nil {
        fmt.Println("Error:", err)
    }
}
```

# Truncate

## Delete

In order to delete a file, there is another simple function, called `os.Remove`, as shown in the following code:

```go
package main

import "os"

func main() {
    if err := os.Remove("file.txt"); err != nil {
        fmt.Println("Error:", err)
    }
}
```

## Move

The `os.Rename` function makes it possible to change a file name and/or its directory. Note that this operation replaces the destination file if it already exists.

The code for changing a file's name or its directory is as follows:

```go
package main

import "os"

func main() {
    if err := os.Rename("file.txt", "../file.txt"); err != nil {
        fmt.Println("Error:", err)
    }
}
```

## Copy

There's no unique function that makes it possible to copy a file, but this can easily be done with a reader and a writer with the `io.Copy` function. The following example shows how to use it to copy from one file to another:

```go
func CopyFile(from, to string) (int64, error) {
    src, err := os.Open(from)
    if err != nil {
        return 0, err
    }
    defer src.Close()
    dst, err := os.OpenFile(to, os.O_WRONLY|os.O_CREATE, 0644)
    if err != nil {
        return 0, err
    }
    defer dst.Close()
    return io.Copy(dst, src)
}
```

## Stats

The `os` package offers the `FileInfo` interface, which returns the metadata of a file, as shown in the following code:

```go
type FileInfo interface {
    Name() string       // base name of the file
    Size() int64        // length in bytes for regular files; system-dependent for others
    Mode() FileMode     // file mode bits
    ModTime() time.Time // modification time
    IsDir() bool        // abbreviation for Mode().IsDir()
    Sys() interface{}   // underlying data source (can return nil)
}
```

The `os.Stat` function returns information about the file with the specified path.

## Changing properties

In order to interact with the filesystem and change these properties, three functions are available:

*   `func Chmod(name string, mode FileMode) error`: Changes the permissions of a file
*   `func Chown(name string, uid, gid int) error`: Changes the owner and group of a file
*   `func Chtimes(name string, atime time.Time, mtime time.Time) error`: Changes the access and modification time of a file

# Third-party packages

The community offers many packages that accomplish all kinds of tasks. We will take a quick look at some of these in this section.

## Virtual filesystems

Files are a struct in Go, a concrete type, and there's no abstraction around them, whereas a file's information is represented by `os.FileInfo`, which is an interface. This is slightly inconsistent, and there have been many attempts to create a full and consistent abstraction on the filesystem, commonly referred to as a *virtual filesystem*.

Two of the most used packages are as follows:

*   `vfs`: [github.com/blang/vfs](https://github.com/blang/vfs)
*   `afero`: [github.com/spf13/afero](https://github.com/spf13/afero)

Even if they are developed separately, they both do the same thing – they define an interface with all the methods of `os.File`, and then they define an interface that implements the function that's available in the `os` package, like creating, opening, and deleting files, and so on.

They offer a version based on `os.File` that's implemented using the standard package, but there's also a memory version that uses data structures that emulate a filesystem. This can be very useful for building a test for any package.

## Filesystem events

Go has some experimental features in the `golang.org/x/` package that are located under Go's GitHub handler ([https://github.com/golang/](https://github.com/golang/)). The `golang.org/x/sys` package is part of this list and includes a subpackage dedicated to Unix system events. This has been used to build a feature that is missing from Go's file functionality and can be really useful – observing a certain path for events on files like creation, deletion, and update.

The two most famous implementations are as follows:

*   `notify`: [github.com/rjeczalik/notify](https://github.com/rjeczalik/notify)
*   `fsnotify`: [github.com/fsnotify/fsnotify](https://github.com/fsnotify/fsnotify)

Both packages expose a function that allows the creation of watchers. Watchers are structures that contain channels that are in charge of delivering file events. They also expose another function that's responsible for terminating/closing the watchers and underlying channels.

# Summary

In this chapter, we looked at an overview of how to execute file operations in Go. In order to locate files, an extensive array of functions are offered by the `filepath` package. These can help you execute all kind of operations, from composing paths to extracting elements from it.

We also looked at how to read an operation using various methods, from the easiest and less memory efficient ones that are located in the `io/ioutil` package to the ones that require an `io.Writer` implementation to read a fixed chunk of bytes. The importance of the ability to peek content, as implemented in the `bufio` package, allows for a whole set of operations like read word or read line, which stop the reading operation when a token is found. There are other interfaces that are satisfied by files that are very useful; for example, `io.Closer` ensures that the resource is released, and `io.Seeker` is used to move the reading cursor around without the need to actually read the file and discard the output.

Writing a slice of bytes to a file can be achieved in different ways – the `io/ioutil` package makes it possible to do so with a function call, while for more complex or more memory-efficient operations, there's the `io.Writer` interface. This makes it possible to write a slice of bytes at a time, and can be used by the `fmt` package to print formatted data. The buffered writing is used to reduce the amount of actual writing on the disk. This is done with a buffer that collects the content, which then transfers it to a disk every time it gets full.

Finally, we saw how to accomplish other file operations on the filesystem (creating, deleting, copying/moving, and changing a file's attributes) and took a look at some of the filesystem-related third-party packages, that is, virtual filesystem abstraction and filesystem events notifications.

The next chapter will be about streams, and will focus on all the instances of readers and writers that are not related to the filesystem.

# Questions

1.  What's the difference between absolute and relative paths?
2.  How do you obtain or change the current working directory?
3.  What are the advantages and downfalls of using `ioutil.ReadAll`?
4.  Why are buffers important for reading operations?
5.  When should you use `ioutil.WriteFile`?
6.  Which operations are available when using a buffered reader that allows peeking?
7.  When is it better to read content using a byte buffer?
8.  How can buffers be used for writing? What's the advantage of using them?

# Handling Streams

This chapter deals with streams of data, extending input and output interfaces beyond the filesystem, and how to implement custom readers and writers to serve any purpose.

It also focuses on the missing parts of the input and output utilities that combine them in several different ways, with the goal being to have full control of the incoming and outgoing data.

The following topics will be covered in this chapter:

*   Streams
*   Custom readers
*   Custom writers
*   Utilities

# Technical requirements

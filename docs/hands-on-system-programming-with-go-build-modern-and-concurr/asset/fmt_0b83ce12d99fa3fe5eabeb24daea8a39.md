
I also recommend installing Visual Studio Code ([https://code.visualstudio.com/](https://code.visualstudio.com/)) with its vscode-go ([https://github.com/Microsoft/vscode-go](https://github.com/Microsoft/vscode-go)) extension, which contains a helper that will install all the tools that are needed to improve the Go development experience.

# Language features

Go is a modern server language with great concurrency primitives and a memory system that is mostly automated. It is considered by some to be a successor to C, and it manages to do so in many scenarios because it's performant, has an extensive standard library, and has a great community that provides many third-party libraries which cover, extend, and improve its functionalities.

# History of Go

Go was created in 2007 in order to try and address Google's engineering problems, and was publicly announced in 2009, reaching version 1.0 in 2012. The major version is still the same (version 1), while the minor version grows (version 1.1, 1.2, and so on) together with its functionalities. This is done to keep Go's promise of compatibility for all major versions. A draft for two new features (generics and error handling), which will probably be included in version 2.0, were presented in 2018.

The minds behind Go are as follows:

* **Robert Griesemer**: A Google researcher who worked on many projects, including V8 JavaScript engine and design, and the implementation of Sawzall.
* **Rob Pike**: A member of the Unix team, the Plan 9 and Inferno OS development team, and the Limbo programming language design team.
* **Ken Thompson**: A pioneer of computer science, the designer of the original Unix, and inventor of B (direct predecessor of C). Ken was also one of the creators and early developers of the Plan 9 operating system.

# Strengths and weaknesses

Go is a very opinionated language; some people love it while some people hate it, mostly due to some of its design choices. Some of the features that have not been well received are as follows:

* Verbose error handling
* Lack of generics
* Missing dependency and version management

The first two points will be addressed with the next major version, while the latter was addressed by the community first (godep, glide, and govendor) and by Google itself with `dep` for the dependencies, and by `gopkg.in` ([http://labix.org/gopkg.in](http://labix.org/gopkg.in)) in terms of version management.

The strengths of the language are countless:

* It's a statically typed language, with all the advantages that this brings, like static type checking.
* It does not require an **integrated development environment** (**IDE**), even if it supports many of them.
* The standard library is really impressive, and it could be the only dependency of many projects.
* It has concurrency primitives (channels and goroutines), which hides the hardest parts of writing asynchronous code that are both efficient and safe.
* It comes with a formatting tool, `gofmt`, that unifies the format of Go code, making other people's code look really familiar.
* It produces binaries, with no dependencies, making deployments fast and easy.
* It's minimalistic and has a few keywords, and the code is really easy to read and understand.
* It's duck typed, with implicit interface definition (*if it walks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck*). This comes in very handy when testing a specific function of a system because it can be mocked.
* It is cross platform, meaning that it's able to produce binaries for architecture and OS that are different from the hosting ones.
* There are a huge amount of third-party packages and so it leaves very little behind in terms of functionality. Each package that's hosted on a public repository is indexed and searchable.

# Namespace

Now, let's see how Go code is organized. The `GOPATH` environment variable determines where the code resides. There are three subdirectories inside this:

* `src` contains all the source code.
* `pkg` contains compiled packages, which are divided into architecture/OS.
* `bin` contains the compiled binaries.

The path under the source folder corresponds to the name of the package (`$GOPATH/src/my/package/name` would be `my/package/name`).

The `go get` command makes it possible to fetch and compile packages using it. `go get` calls to `http://package_name?go-get=1` and if it finds the `go-import` meta tag, it uses this to fetch the package. The tag should contain the package name, the VCS that was used, and the repository URL, all separated by a space. Let's take a look at an example:

```
<meta name="go-import" content="package-name vcs repository-url">
```

After `go get` downloads a package, it tries to do the same for the other packages it can't resolve recursively until all the necessary source code is available.

Each file starts with a package definition, that is, `package package_name`, that needs to be the same for all the files in a directory. If the package produces a binary, the package is `main`.

# Imports and exporting symbols

The package declaration is followed by a series of `import` statements that specify the packages that are needed.

Importing packages that are not used (unless they are ignored) is a compilation error, which is why the Go formatting tool, `gofmt`, removes unused packages. There are experimental or community tools like `goimports` ([https://godoc.org/golang.org/x/tools/cmd/goimports](https://godoc.org/golang.org/x/tools/cmd/goimports)) or `goreturns` ([https://github.com/sqs/goreturns](https://github.com/sqs/goreturns)) that also add missing imports to a Go file. It's important to avoid having circular dependencies because they will not compile.

Since circular dependencies are not allowed, the packages need to be designed differently to other languages. In order to break the circular dependency, it is good practice to export functionalities from a package or replace the dependency with an interface.

Go reduces all the symbol visibility to a binary model – exported and not exported – unlike many other languages, which have intermediate levels. For each package, all the symbols starting with a capital letter are exported, while everything else is used only inside the package. The exported values can also be used by other packages, while the unexported ones can only be used in the package itself.

An exception is made if one of the package path elements is `internal` (for example, `my/package/internal/pdf`). This limits itself and its subpackages to be imported only by the nearby packages (for example, `my/package`). This is useful if there are a lot of unexported symbols and you want to break them down into subpackages, while preventing other packages from using it, making basically private subpackages. Take a look at the following list of internal packages:

* `my/package/internal`
* `my/package/internal/numbers`
* `my/package/internal/strings`

These can only be used by `my/package`, and cannot be imported by any other package, including `my`.

The import can have different forms. The standard import form is the complete package name:

```
import "math/rand"
...
rand.Intn
```

A named import replaces the package name with a custom name, which must be used when referencing the package:

```
import r "math/rand"
...
r.Intn
```

The same package import makes the symbol available with no namespace:

```
import . "math/rand"
...
Intn
```

Ignored imports are used to import packages without you having to use them. This makes it possible to execute the `init` function of the package without referencing the package in your code:

```
import _ "math/rand"  
// still executes the rand.init function
```

# Type system

The Go type system defines a series of basic types, including bytes, strings and buffers, composite types like slices or maps, and custom types that have been defined by the application.

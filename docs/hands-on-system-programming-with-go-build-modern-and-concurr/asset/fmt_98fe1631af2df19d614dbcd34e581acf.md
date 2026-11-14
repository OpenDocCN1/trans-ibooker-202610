
```go
var data = []struct {
    Name  string
    Score int
}{
    {"Michelangelo", 30},
    {"Donatello", 50},
    {"Leonardo", 80},
    {"Raffaello", 100},
}
tpl, err := template.New("question-answer").Parse(`{{range .}}
{{.Name}} scored {{.Score}}. He did {{if lt .Score 50}}bad{{else if lt .Score 75}}okay{{else if lt .Score 90}}good{{else}}great{{end}}
{{end}}`)
if err != nil {
    log.Fatalln("Error:", err)
}
if err = tpl.Execute(os.Stdout, data); err != nil {
    log.Fatalln("Error:", err)
}
```

The full example is available here: [https://play.golang.org/p/eBKDcJ47rPU](https://play.golang.org/p/eBKDcJ47rPU).

We will explore functions in more detail in the next section.

# Template functions

Functions are an important part of the template engine and there are many built-in functions, such as comparison (`eq`, `lt`, `gt`, `le`, `ge`) or logical (`AND`, `OR`, `NOT`). Functions are called by their names, followed by arguments using space as separator. The function used in the previous example, `lt a b`, means `lt(a,b)`. When the functions are more nested, it's required to wrap functions and arguments in parentheses. For instance, the `not lt a b` statement means that the `X` function has three arguments, `not(lt, a, b)`. The correct version is `not (lt a b)`, which tells the template that the elements in the parentheses need to be solved first.

When creating a template, custom functions can be assigned to it with the `Funcs` method and can be used in the template. This is very useful, as we can see in this example:

```go
var data = struct {
    Name, Surname, Occupation, City string
}{
    "Bojack", "Horseman", "Actor", "Los Angeles",
}
tpl, err := template.New("question-answer").Funcs(template.FuncMap{
    "upper": func(s string) string { return strings.ToUpper(s) },
    "lower": func(s string) string { return strings.ToLower(s) },
}).Parse(`{{.Name}} {{.Surname}} - {{lower .Occupation}} from {{upper .City}}`)
if err != nil {
    log.Fatalln("Error:", err)
}
if err = tpl.Execute(os.Stdout, data); err != nil {
    log.Fatalln("Error:", err)
}
```

The full example is available here: [https://play.golang.org/p/DdoKEOixDDB](https://play.golang.org/p/DdoKEOixDDB).

The `|` operator can be used to link the output of a statement to the input of another statement, similar to how it happens in the Unix shell. For instance, the `{{"put" | printf "%s%s" "out" | printf "%q"}}` statement will produce `"output"`.

# RPC servers

**Remote Procedure Call (RPC)** is a method of calling the execution of an application functionality from another system, using the TCP protocol. Go has native support for RPC servers.

## Defining a service

The Go RPC server permits us to register any Go type, along with its methods. This exposes the methods with the RPC protocol and enables us to call them by name from a remote client. Let's create a helper for keeping track of our progress in the book as we are reading:

```go
// Book represents a book entry
type Book struct {
    ISBN   string
    Title  string
    Author string
    Year   int
    Pages  int
}

// ReadingList keeps tracks of books and pages read
type ReadingList struct {
    Books    []Book
    Progress []int
}
```

First, let's define a small helper method called `bookIndex` that returns the index of a book using its identifier—the ISBN:

```go
func (r *ReadingList) bookIndex(isbn string) int {
    for i := range r.Books {
        if isbn == r.Books[i].ISBN {
            return i
        }
    }
    return -1
}
```

Now, we can define the operation that `ReadingList` will be capable of. It should be able to add and remove books:

```go
// AddBook checks if the book is not present and adds it
func (r *ReadingList) AddBook(b Book) error {
    if b.ISBN == "" {
        return ErrISBN
    }
    if r.bookIndex(b.ISBN) != -1 {
        return ErrDuplicate
    }
    r.Books = append(r.Books, b)
    r.Progress = append(r.Progress, 0)
    return nil
}

// RemoveBook removes the book from list and forgets its progress
func (r *ReadingList) RemoveBook(isbn string) error {
    if isbn == "" {
        return ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return ErrMissing
    }
    // replace the deleted book with the last of the list
    r.Books[i] = r.Books[len(r.Books)-1]
    r.Progress[i] = r.Progress[len(r.Progress)-1]
    // shrink the list of 1 element to remove the duplicate
    r.Books = r.Books[:len(r.Books)-1]
    r.Progress = r.Progress[:len(r.Progress)-1]
    return nil
}
```

It should also be able to read and alter a book's progress:

```go
// GetProgress returns the progress of a book
func (r *ReadingList) GetProgress(isbn string) (int, error) {
    if isbn == "" {
        return -1, ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return -1, ErrMissing
    }
    return r.Progress[i], nil
}
```

Then, `SetProgress` changes the progress of a book, as follows:

```go
func (r *ReadingList) SetProgress(isbn string, pages int) error {
    if isbn == "" {
        return ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return ErrMissing
    }
    if p := r.Books[i].Pages; pages > p {
        pages = p
    }
    r.Progress[i] = pages
    return nil
}
```

`AdvanceProgress` adds pages to the progress of a book:

```go
func (r *ReadingList) AdvanceProgress(isbn string, pages int) error {
    if isbn == "" {
        return ErrISBN
    }
    i := r.bookIndex(isbn)
    if i == -1 {
        return ErrMissing
    }
    if p := r.Books[i].Pages - r.Progress[i]; p < pages {
        pages = p
    }
    r.Progress[i] += pages
    return nil
}
```

The error variables we are using in these functions are defined as follows:

```go
// List of errors
var (
    ErrISBN      = fmt.Errorf("missing ISBN")
    ErrDuplicate = fmt.Errorf("duplicate book")
    ErrMissing   = fmt.Errorf("missing book")
)
```

## Creating the server

Now we have the service that we can use to create an RPC server very easily. However, the type used has to respect some rules for its methods to make them available:

*   The method's type and method itself are both exported.
*   The method has two arguments, both exported.
*   The second argument is a pointer.
*   The method returns an error.

The method should look something like this: `func (t *T) Method(in T1, out *T2) error.`

The next step is create a wrapper for the `ReadingList` that satisfies these rules:

```go
// ReadingService adapts ReadingList for RPC
type ReadingService struct {
    ReadingList
}

// sets the success pointer value from error
func setSuccess(err error, b *bool) error {
    *b = err == nil
    return err
}
```

We can redefine the book, add and remove functions using `Book`, which is an exported type, and built-in types:

```go
func (r *ReadingService) AddBook(b Book, success *bool) error {
    return setSuccess(r.ReadingList.AddBook(b), success)
}

func (r *ReadingService) RemoveBook(isbn string, success *bool) error {
    return setSuccess(r.ReadingList.RemoveBook(isbn), success)
}
```

For the progress, we have two inputs (`isbn` and `pages`), so we have to define a structure that contains both, since the input must be a single argument:

```go
func (r *ReadingService) GetProgress(isbn string, pages *int) (err error) {
    *pages, err = r.ReadingList.GetProgress(isbn)
    return err
}

type Progress struct {
    ISBN  string
    Pages int
}

func (r *ReadingService) SetProgress(p Progress, success *bool) error {
    return setSuccess(r.ReadingList.SetProgress(p.ISBN, p.Pages), success)
}

func (r *ReadingService) AdvanceProgress(p Progress, success *bool) error {
    return setSuccess(r.ReadingList.AdvanceProgress(p.ISBN, p.Pages), success)
}
```

The defined type can be registered and used in an RPC server, which will use `rpc.HandleHTTP` to register the HTTP handler for the incoming RPC messages.

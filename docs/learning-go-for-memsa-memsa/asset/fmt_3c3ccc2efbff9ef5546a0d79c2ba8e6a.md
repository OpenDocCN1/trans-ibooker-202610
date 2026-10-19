
`TestMain` is useful in two common situations:

* When you need to set up data in an external repository, such as a database
* When the code being tested depends on package-level variables that need to be initialized

As I’ve mentioned before (and will again!), you should avoid package-level variables in your programs. They make it hard to understand how data flows through your program. If you are using `TestMain` for this reason, consider refactoring your code.

The `Cleanup` method on `*testing.T` is used to clean up temporary resources created for a single test. This method has a single parameter: a function with no input parameters or return values. The function runs when the test completes. For simple tests, you can achieve the same result by using a `defer` statement, but `Cleanup` is useful when tests rely on helper functions to set up sample data, as you see in [Example 2-1](#ch02.html#EX15_1). It’s fine to call `Cleanup` multiple times. Just like `defer`, the functions are invoked in last-added, first-called order.

### Example 2-1. Using `t.Cleanup`

```
// createFile is a helper function called from multiple tests
func createFile(t *testing.T) (_ string, err error) {
    f, err := os.Create("tempFile")
    if err != nil {
        return "", err
    }
    defer func() {
        err = errors.Join(err, f.Close())
    }()
    // write some data to f
    t.Cleanup(func() {
        os.Remove(f.Name())
    })
    return f.Name(), nil
}

func TestFileProcessing(t *testing.T) {
    fName, err := createFile(t)
    if err != nil {
        t.Fatal(err)
    }
    // do testing, don't worry about cleanup
}
```

If your test uses temporary files, you can avoid writing cleanup code by taking advantage of the `TempDir` method on `*testing.T`. This method creates a new temporary directory every time it is invoked and returns the full path of the directory. It also registers a handler with `Cleanup` to delete the directory and its contents when the test has completed. You can use it to rewrite the previous example:

```
// createFile is a helper function called from multiple tests
func createFile(tempDir string) (_ string, err error) {
    f, err := os.CreateTemp(tempDir, "tempFile")
    if err != nil {
        return "", err
    }
    defer func() {
        err = errors.Join(err, f.Close())
    }()
    // write some data to f
    return f.Name(), nil
}

func TestFileProcessing(t *testing.T) {
    tempDir := t.TempDir()
    fName, err := createFile(tempDir)
    if err != nil {
        t.Fatal(err)
    }
    // do testing, don't worry about cleanup
}
```

## Test Helpers

Functions like `createFile` are sometimes referred to as *test helpers* since they set up or tear down the state needed to properly test code, but do no testing themselves. If something goes wrong it is usually a fatal error, as the test cannot continue without valid initial conditions. To mark the function as a test helper, it is idiomatic to pass a `*testing.T` instance to these functions and add a call to `t.Helper` as the first line:

```
func TestFileProcessingWithCreateFileHelper(t *testing.T) {
    fName := createFileHelper(t)
    // do testing, don't worry about cleanup
    if !strings.Contains(fName, "tempFile") {
        t.Error("unexpected name")
    }
}

// createFileHelper is a helper function called from multiple tests
func createFileHelper(t *testing.T) string {
    t.Helper()
    tempDir := t.TempDir()
    f, err := os.CreateTemp(tempDir, "tempFile")
    if err != nil {
        t.Fatal(err)
    }
    // write some data to the file
    if err := f.Close(); err != nil {
        t.Fatal(err)
    }
    return f.Name()
}
```

When `t.Helper` is called, it modifies the output of any calls to `t.Error` or `t.Fatal` within the test helper. Rather than print the filename and line number of the line in the test helper, the filename and line number of the line in the test function that called the test helper is displayed. The `sample_code/helper` directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/helper) has an example. It is also available on [the Go Playground](https://go.dev/play/p/zboR2P3V4bl). Run the test with the `t.Helper()` line commented and then uncommented and look at the difference in the error output.

Never test any business logic within a test helper; only use `t.Error` or `t.Fatal` if setup or teardown failed for some reason.

## Testing with Environment Variables

It’s a common (and very good) practice to configure applications with environment variables. To simplify writing tests for code that depends on environment variables, Go provides a helper method on `testing.T`. Call `t.Setenv` to register a value for an environment variable for your test. Behind the scenes, it calls `Cleanup` to revert the environment variable to its previous state when the test exits:

```
// assume ProcessEnvVars is a function that processes environment variables
// and returns a struct with an OutputFormat field
func TestEnvVarProcess(t *testing.T) {
    t.Setenv("OUTPUT_FORMAT", "JSON")
    cfg := ProcessEnvVars()
    if cfg.OutputFormat != "JSON" {
        t.Error("OutputFormat not set correctly")
    }
    // value of OUTPUT_FORMAT is reset when the test function exits
}
```

### Note

While it is good to use environment variables to configure your application, it is also good to make sure that most of your code is entirely unaware of them. Be sure to copy the values of environment variables into configuration structs before your program starts its work, in your `main` function or soon afterward. Doing so makes it easier to reuse and test code, since *how* the code is configured has been abstracted away from *what* the code is doing.

Rather than writing this code yourself, you should strongly consider using a third-party configuration library, like [Viper](https://oreil.ly/-RUA-) or [envconfig](https://oreil.ly/rhGYk). Also, look at [GoDotEnv](https://oreil.ly/sN2Sp) as a way to store environment variables in `*.env` files for development or continuous integration machines.

## Storing Sample Test Data

As `go test` walks your source code tree, it uses the current package directory as the current working directory. If you want to use sample data to test functions in a package, create a subdirectory named `testdata` to hold your files. Go reserves this directory name as a place to hold test files. When reading from `testdata`, always use a relative file reference. Since `go test` changes the current working directory to the current package, each package accesses its own `testdata` via a relative file path.

### Tip

The [`text` package](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/text) in the `sample_code` directory of the Chapter 15 repository demonstrates how to use `testdata`.

## Caching Test Results

As you learned in Chapter 10, Go caches compiled packages if they haven’t changed. Go also caches test results when running tests across multiple packages if they have passed and their code hasn’t changed. The tests are recompiled and rerun if you change any file in the package or in the `testdata` directory. You can also force tests to always run if you pass the flag `-count=1` to `go test`.

## Testing Your Public API

The tests that you’ve written are in the same package as the production code. This allows you to test both exported and unexported functions.

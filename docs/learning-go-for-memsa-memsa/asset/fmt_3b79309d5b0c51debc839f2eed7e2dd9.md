
```
func TestDoMath(t *testing.T) {
    result, err := DoMath(2, 2, "+")
    if result != 4 {
        t.Error("Should have been 4, got", result)
    }
    if err != nil {
        t.Error("Should have been nil error, got", err)
    }
    result2, err2 := DoMath(2, 2, "-")
    if result2 != 0 {
        t.Error("Should have been 0, got", result2)
    }
    if err2 != nil {
        t.Error("Should have been nil error, got", err2)
    }
    // and so on...
}
```

Let’s replace this repetition with a table test. First, declare a slice of anonymous structs. The struct contains fields for the name of the test, the input parameters, and the return values. Each entry in the slice represents another test:

```
data := []struct {
    name     string
    num1     int
    num2     int
    op       string
    expected int
    err      error
}{
    {"addition", 2, 2, "+", 4, nil},
    {"subtraction", 2, 2, "-", 0, nil},
    {"multiplication", 2, 2, "*", 4, nil},
    {"division", 2, 2, "/", 1, nil},
    {"bad_division", 2, 0, "/", 0, ErrDivZero},
}
```

Next, loop over each test case in `data`, invoking the `Run` method each time. This is the line that does the magic. You pass two parameters to `Run`, a name for the subtest and a function with a single parameter of type `*testing.T`. Inside the function, you call `DoMath` by using the fields of the current entry in `data`, using the same logic over and over. When you run these tests, you’ll see that not only do they pass, but when you use the `-v` flag, each subtest also now has a name:

```
for _, d := range data {
    t.Run(d.name, func(t *testing.T) {
        result, err := DoMath(d.num1, d.num2, d.op)
        if result != d.expected {
            t.Errorf("Expected %d, got %d", d.expected, result)
        }
        if !errors.Is(err, d.err) {
            t.Errorf("expected %v, got %v", d.err, err)
        }
    })
}
```

### Tip

Comparing error messages in tests is fragile, because there may not be any compatibility guarantees on the message text. If you are testing code that is using `errors.New` and `fmt.Errorf` to make errors, you have two options: rewrite your code to use custom error types and sentinel errors, or compare the messages and hope for the best. It’s better to re-write your code.

# Running Tests Concurrently

By default, unit tests are run sequentially. Since each unit test is supposed to be independent from every other unit test, they make ideal candidates for concurrency. To make a unit test run concurrently with other tests, call the `Parallel` method on `*testing.T` as the first line in your test:

```
func TestMyCode(t *testing.T) {
    t.Parallel()
    // rest of test goes here
}
```

Parallel tests run concurrently with other tests marked as parallel. You can also call `Parallel` from within a table test:

```
func TestParallelTable(t *testing.T) {
    data := []struct {
        name   string
        input  int
        output int
    }{
        {"a", 10, 20},
        {"b", 30, 40},
        {"c", 50, 60},
    }
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            t.Parallel()
            t.Log(d.input, d.output)
            out := toTest(d.input)
            if out != d.output {
                t.Error("didn't match", out, d.output)
            }
        })
    }
}
```

You can run this code on [The Go Playground](https://oreil.ly/b0S4n) or in the `sample_code/parallel` directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/parallel).

The advantage of parallel tests is they can speed up long-running test suites. There are some disadvantages, though. If you have multiple tests that rely on the same shared mutable state, do not mark them as parallel, because you will get inconsistent results. (But after all of my warnings, you don’t have any shared mutable state in your application, right?) Also be aware that your test will panic if you mark it as parallel and use the `Setenv` method in your test function.

### Note

Go 1.22 (released in February, 2024) changed the lifecycle of the index and value variables in `for` loops. The compiler now creates a new copy of the index and value variables on each iteration if there’s any chance they could be captured by a closure. This eliminated bugs in parallel tests that were using shared index and value state. If you are still using Go 1.21 or earlier (or have the Go version in the `go` directive in your `go.mod` file set to 1.2.1 or earlier), I strongly recommend upgrading to take advantage of this improved behavior.

Now that you have a way to run lots of tests, you’ll learn about code coverage to find out what your tests are testing.

# Exercises

These exercises help you practice the basics of Go testing. They are found in the `exercises` directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercises). Solutions are in the `exercise-solutions` directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercise-solutions).

1. In the `ex1` [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex1), there is a file called `calculator.go`, with a function called `Average`. Create a `calculator_test.go` file with a function to test `Average`. If you find a bug in the function, fix the bug and confirm that the test passes.

2. In the `ex2` [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex2), there is a file called `temperature.go` with a function called `Convert`. Create a `temperature_test.go` file with a function that uses a table test to validate all inputs and possible errors. If you find a bug in the function, fix the bug and confirm that the test passes.

3. In the `ex3` [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex3), there is a file called `counter.go` with a function called `TotalFile`. Create a `counter_test.go` file with a function to test `TotalFile`. Use a helper function to create both good and bad sample data.

# Checking Your Code Coverage

Code coverage is a very useful tool for knowing whether you’ve missed any obvious cases. However, reaching 100% code coverage doesn’t guarantee that there aren’t bugs in your code for some inputs. First you’ll see how `go test` displays code coverage and then you’ll look at the limitations of relying on code coverage alone.

Adding the `-cover` flag to the `go test` command calculates coverage information and includes a summary in the test output. If you include a second flag `-coverprofile`, you can save the coverage information to a file. Let’s go back to the `sample_code/table` directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/table) and gather code coverage information:

```
$ go test -v -cover -coverprofile=c.out
```

If you run your table test with code coverage, the test output now includes a line that indicates the amount of test code coverage, 87.5%. That’s good to know, but it’d be more useful if you could see what you missed. The `cover` tool included with Go generates an HTML representation of your source code with that information:

```
$ go tool cover -html=c.out
```

When you run it, your web browser opens and shows you a page with code coverage information using a dark mode color scheme. Every file that’s tested appears in the combo box in the upper left. The source code is in one of three colors. Gray is used for lines of code that aren’t testable, green is used for code that’s been covered by a test, and red is used for code that hasn’t been tested. This reliance on color is unfortunate for those who have red-green color blindness (and those whose publishers don’t want to print large blocks of black ink). To address this issue, I have created a [fork of the cover tool](https://github.com/jonbodner/coverlight). Install and run it with:

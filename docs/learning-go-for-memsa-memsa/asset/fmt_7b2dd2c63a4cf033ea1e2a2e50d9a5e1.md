
It’s important to ensure that calls to HTTP services send the right requests and handle responses properly. While integration testing is essential for validating a multi-service deployment, it is often slow. Quick-running unit tests for HTTP clients provide confidence during development. The Go standard library includes the [`net/http/httptest`](https://oreil.ly/0c_MX) package, which makes it easy to stub HTTP services. Let’s go back to the `sample_code/solver` directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/solver) and provide an implementation of `MathSolver` that calls an HTTP service to evaluate expressions:

```
type RemoteSolver struct {
    MathServerURL string
    Client        *http.Client
}

func (rs RemoteSolver) Resolve(ctx context.Context, expression string)
                              (float64, error) {
    req, err := http.NewRequestWithContext(ctx, http.MethodGet,
        rs.MathServerURL+"?expression="+url.QueryEscape(expression), nil)
    if err != nil {
        return 0, err
    }
    resp, err := rs.Client.Do(req)
    if err != nil {
        return 0, err
    }
    defer resp.Body.Close()
    contents, err := io.ReadAll(resp.Body)
    if err != nil {
        return 0, err
    }
    if resp.StatusCode != http.StatusOK {
        return 0, &RequestErr{
            StatusCode: resp.StatusCode,
            Contents:   string(contents),
        }
    }
    result, err := strconv.ParseFloat(string(contents), 64)
    if err != nil {
        return 0, err
    }
    return result, nil
}
```

Now let’s see how to use the `httptest` library to test this code without standing up a server. The code is in the `TestRemoteSolver_Resolve` function in `sample_code/solver/remote_solver_test.go` in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/blob/main/sample_code/solver/remote_solver_test.go), but here are the highlights. First, you want to make sure that the data that’s passed into the function arrives on the server. In your test function, define a type called `info` to hold your input and output and a variable called `io` that is assigned the current input and output:

```
type info struct {
    expression string
    code       int
    body       string
}
var io info
```

Next, set up your fake remote server and use it to configure an instance of `RemoteSolver`:

```
server := httptest.NewServer(
    http.HandlerFunc(func(rw http.ResponseWriter, req *http.Request) {
        // validate the incoming request to make sure the client is
        // sending the expected data
        if req.URL.Query().Get("expression") != io.expression {
            rw.WriteHeader(http.StatusBadRequest)
            return
        }
        rw.WriteHeader(io.code)
        rw.Write([]byte(io.body))
    }))
defer server.Close()
rs := RemoteSolver{
    MathServerURL: server.URL,
    Client:        server.Client(),
}
```

The `httptest.NewServer` function creates and starts an HTTP server on a random unused port. You need to provide an `http.Handler` implementation to process the request. Since this is a server, you must close it when the test completes. The `server` instance has its URL specified in the `URL` field of the `server` instance and a preconfigured `http.Client` for communicating with the test server. Pass these into `RemoteSolver`.

The rest of the function works like every other table test that you’ve seen:

```
data := []struct {
    name   string
    io     info
    result float64
    err    error
}{
    {"case1", info{"2 + 2 * 10", http.StatusOK, "22"}, 22, nil},
    {"case2", info{"( 2 + 2 ) * 10", http.StatusOK, "40"}, 40, nil},
    {"case3", info{"( 2 + 2 * 10", http.StatusBadRequest,
                    "invalid expression: ( 2 + 2 * 10"}, 0,
        &RequestErr{
            StatusCode: http.StatusBadRequest,
            Contents:   "invalid expression: ( 2 + 2 * 10",
        }},
}
for _, d := range data {
    t.Run(d.name, func(t *testing.T) {
        io = d.io
        result, err := rs.Resolve(t.Context(), d.io.expression)
        if result != d.result {
            t.Errorf("io `%f`, got `%f`", d.result, result)
        }
        if !errors.Is(err, d.err) {
            t.Errorf("expected error %v, got %v", d.err, err)
        }
    })
}
```

The variable `io` has been captured by two closures: the one for the stub server and the one for running each test. You write to it in one closure and read it in the other. This is a bad idea in production code, but it works well in test code within a single function.

# Using Integration Tests and Build Tags

Even though `httptest` provides a way to avoid testing against external services, you should still write *integration tests*, automated tests that connect to other services. These validate that your understanding of the service’s API is correct. The challenge is figuring out how to group your automated tests; you want to run integration tests only when the support environment is present. Also, integration tests tend to be slower than unit tests, so they are usually run less frequently.

In Chapter 11, I covered build tags, which the Go compiler uses to control when a file is compiled. While they are primarily intended to allow developers to write code that’s intended only for a specific operating system, CPU, or Go version, you can take advantage of the ability to specify custom build tags to control when integration tests are compiled and run.

Let’s try this out with the math-solving project. Use [Docker](https://oreil.ly/5FWcb) to download a server implementation with `docker pull jonbodner/math-server` and then run the server locally on port 8080 with `docker run -p 8080:8080` `jonbodner/math-server`. (The `sample_code/solver` directory in the Chapter 15 repository has a Docker Compose file, so if you have cloned it locally, you can run `docker compose up`.)

###### Note

If you don’t have Docker installed or if you want to build the code for yourself, you can find the math-server code on [GitHub](https://oreil.ly/yjMzc).

You need to write an integration test to make sure that your `Resolve` method properly communicates with the math server. The `sample_code/solver/remote_solver_integration_test.go` file in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/blob/main/sample_code/solver/remote_solver_integration_test.go) has a complete test in the `TestRemoteSolver_ResolveIntegration` function. The test looks like every other table test that you’ve written. The interesting thing is the first line of the file, separated from the package declaration by a newline:

```
//go:build integration
```

To run your integration test alongside the other tests you’ve written, use this:

```
$ go test -tags integration -v ./...
```

Some people in the Go community prefer to use environment variables rather than build tags to create groups of integration tests. Peter Bourgon describes the concept in a blog post with the unsubtle title [“Don’t Use Build Tags for Integration Tests”](https://oreil.ly/BXSnu). His argument is that it’s hard to find out what build tags to set in order to run integration tests. An explicit check for an environment variable in each integration test, combined with a detailed message in a `t.Skip` method call, makes it clear that tests aren’t being run and how to run them. In the end, it’s a trade-off between verbosity and discoverability. Feel free to use either technique.

# Exercises

Now that you’ve learned about using mocks and stubs, complete these exercises to apply that knowledge to a sample application.

1. In the `ex7` directory, the `notifier.go` file contains a type called `NotificationService` with a method called `Notify`. Use a function field stub to write a table test to make sure this code properly handles correct data, invalid input, and a failed send.

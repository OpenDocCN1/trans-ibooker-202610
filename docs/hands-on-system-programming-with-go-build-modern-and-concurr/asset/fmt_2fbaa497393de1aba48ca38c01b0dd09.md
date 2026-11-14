
# Concurrency Patterns Implementation

## Simple Implementation with Private Channels

A very simple implementation of this scenario can be obtained using private channels between the `main` goroutine and the senders. We can define a very simple structure that carries both messages and a `Wait` channel. It will have two methods – one for marking the transaction as done and another one that waits for such a signal – when it uses a channel underneath. The following method shows this:

```go
type msg struct {
    value string
    done  chan struct{}
}

func (m *msg) Wait() {
    <-m.done
}

func (m *msg) Done() {
    m.done <- struct{}{}
}
```

## Generator Function: `send`

We can create a source of messages with a generator. We can use a random delay with the `send` operation. After each send, we wait for the signal that is obtained by calling the `Done` method. We always use context to keep everything free from leaks:

```go
func send(ctx context.Context, v string) <-chan msg {
    ch := make(chan msg)
    go func() {
        done := make(chan struct{})
        for i := 0; ; i++ {
            time.Sleep(time.Duration(float64(time.Second/2) * rand.Float64()))
            m := msg{fmt.Sprintf("%s msg-%d", v, i), done}
            select {
            case <-ctx.Done():
                close(ch)
                return
            case ch <- m:
                m.Wait()
            }
        }
    }()
    return ch
}
```

## Fan-in: `merge` Function

We can use a fan-in to put all of the channels into one, singular channel:

```go
func merge(ctx context.Context, sources ...<-chan msg) <-chan msg {
    ch := make(chan msg)
    go func() {
        <-ctx.Done()
        close(ch)
    }()
    for i := range sources {
        go func(i int) {
            for {
                select {
                case v := <-sources[i]:
                    select {
                    case <-ctx.Done():
                        return
                    case ch <- v:
                    }
                }
            }
        }(i)
    }
    return ch
}
```

## Main Application

The main application will be receiving from the merged channel until it's closed. When it receives one message from each channel, the channel will be blocked, waiting for the `Done` method signal to be called by the main goroutine.

This specific configuration will allow the main goroutine to receive just one message from each channel. When the message count reaches the number of goroutines, we can call `Done` from the main goroutine and reset the list so that the other goroutines will be unlocked and be able to send messages again:

```go
func main() {
    ctx, canc := context.WithTimeout(context.Background(), time.Second)
    defer canc()
    sources := make([]<-chan msg, 5)
    for i := range sources {
        sources[i] = send(ctx, fmt.Sprint("src-", i))
    }
    msgs := make([]msg, 0, len(sources))
    start := time.Now()
    for v := range merge(ctx, sources...) {
        msgs = append(msgs, v)
        log.Println(v.value, time.Since(start))
        if len(msgs) == len(sources) {
            log.Println("*** done ***")
            for _, m := range msgs {
                m.Done()
            }
            msgs = msgs[:0]
            start = time.Now()
        }
    }
}
```

## Running the Application

Running the application will result in all the goroutines sending a message to the main one once. Each of them will be waiting for everyone to send their message. Then, they will start sending messages again. This results in messages being sent in rounds, as expected.

## Summary

In this chapter, we looked at some specific concurrency patterns for our applications. We learned that generators are functions that return channels, and also feed such channels with data and close them when there is no more data. We also saw that we can use a `context` to allow the generator to exit early.

Next, we focused on pipelines, which are stages of execution that use channels for communication. They can either be **source**, which doesn't require any input; **destination**, which doesn't return a channel; or **intermediate**, which receives a channel as input and returns one as output.

Another pattern is the multiplexing and demultiplexing one, which consists of spreading a channel to different goroutines and combining several channels into one. It is often referred to as *fan-out fan-in*, and it allows us to execute different operations concurrently on a set of data.

Finally, we learned how to implement a better version of the rate limiter called **leaky bucket**, which limits the number of requests in a specific amount of time. We also looked at the sequencing pattern, which uses a private channel to signal to all of the sending goroutines when they are allowed to send data again.

In the next chapter, we are going to introduce the first of two extra topics that were presented in the *Sequencing* section. It is here that we will demonstrate how to use reflection to build generic code that adapts to any user-provided type.

## Questions

1.  What is a generator? What are its responsibilities?
2.  How could you describe a pipeline?
3.  What type of stage gets a channel and returns one?
4.  What is the difference between fan-in and fan-out?

# Section 5: A Guide to Using Reflection and CGO

This section focuses on two tools that are very controversial – reflection, which allows the creation of generic code but has a great cost in terms of performance, and CGO, which allows the use of C code in a Go application but makes it more complex to debug and control the application.

This section consists of the following chapters:

*   **Chapter 15**, *Using Reflection*
*   **Chapter 16**, *Using CGO*

# Using Reflection

This chapter is about **reflection**, a tool that allows an application to inspect its own code, overcoming some of the limitations imposed by Go static typing and its lack of generics. This can be very helpful, for instance, for producing packages that are capable of handling any type of input that they receive.

The following topics will be covered in this chapter:

*   Understanding interfaces and type assertions
*   Learning about interaction with basic types
*   Using reflection with complex types
*   Evaluating the cost of reflection
*   Learning the best practices of reflection usage

## Technical Requirements

This chapter requires Go to be installed and your favorite editor to be set up. For more information, refer to **Chapter 3**, *An Overview of Go*.

## What's Reflection?

Reflection is a very powerful feature that allows **meta-programming**, the capability of an application to examine its own structure. It's very useful to analyze the types in an application at runtime, and it is used in many encoding packages such as `JSON` and `XML`.

## Type Assertions

We briefly mentioned how type assertions work in **Chapter 3**, *An Overview of Go*. A type assertion is an operation that allows us to go from interface to concrete type and vice versa. It takes the following form:

```go
// unsafe assertion
v := SomeVar.(SomeType)

// safe assertion
v, ok := SomeVar.(SomeType)
```

The first version is unsafe, and it assigns a value to a single variable.

Using assertion as an argument of a function also counts as unsafe. This type of operation panics if the assertion is wrong:

```go
func main() {
    var a interface{} = "hello"
    fmt.Println(a.(string)) // ok
    fmt.Println(a.(int))    // panics!
}
```

A full example is available here: <https://play.golang.org/p/hNN87SuprGR>.

The second version uses a Boolean `ok` as second value, and it will show the success of the operation. If the assertion is not possible, the first value will always be a zero value for the asserted type.

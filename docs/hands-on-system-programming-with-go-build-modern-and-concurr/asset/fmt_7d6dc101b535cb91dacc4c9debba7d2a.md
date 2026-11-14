
## Switch Statement

```go
switch tier {                        // switch statement
case 1:                              // case statement
    fmt.Println("T-shirt")
    if age < 18{
        break                        // exits the switch block
    }
    fallthrough                      // executes the next case
case 2:
    fmt.Println("Mug")
    fallthrough                      // executes the next case 
case 3:
    fmt.Println("Sticker pack")    
default:                             // executed if no case is satisfied
    fmt.Println("no reward")
}
```

# Looping

The `for` statement is the only looping statement in Go. This requires that you specify three expressions, separated by `;`:

*   A short declaration or an assignment to an existing variable
*   A condition to verify before each iteration
*   An operation to execute at the end of the iteration

All of these statements can be optional, and the absence of a condition means that it is always `true`. A `break` statement interrupts the loop's execution, while `continue` skips the current iteration and proceeds with the next one:

```go
for {                    // infinite loop
    if condition {
        break            // exit the loop
    }
}

for i < 0 {              // loop with condition
    if condition {
        continue         // skip current iteration and execute next    
    }
}

for i:=0; i < 10; i++ {  // loop with declaration, condition and operation 
}
```

When a combination of `switch` and `for` are nested, the `continue` and `break` statements refer to the inner flow control statement.

An outer loop or condition can be labelled using a `name:` expression, whereas the name is its identifier, and both `break` and `continue` can be followed by the name in order to specify where to intervene, as shown in the following code:

```go
label:
    for i := a; i<a+2; i++ {
        switch i%3 {
        case 0:
            fmt.Println("divisible by 3")
            break label                          // this break the outer for loop
        default:
            fmt.Println("not divisible by 3")
        }
    }
```

# Exploring Built-in Functions

We already listed some of the built-in functions that are used to initialize some variables, that is, `make` and `new`. Now, let's go over each function and check out what they do:

*   `func append(slice []Type, elems ...Type) []Type`: This function appends elements to the end of a slice. If the underlying array is full, it reallocates the content to a bigger slice before appending.
*   `func cap(v Type) int`: Returns the number elements of an array, or of the underlying array if the argument is a slice.
*   `func close(c chan<- Type)`: Closes a channel.
*   `func complex(r, i FloatType) ComplexType`: Given two floating points, this returns a complex number.
*   `func copy(dst, src []Type) int`: Copies elements from a slice to another.
*   `func delete(m map[Type]Type1, key Type)`: Removes an entry from a map.
*   `func imag(c ComplexType) FloatType`: Returns the imaginary part of a complex number.
*   `func len(v Type) int`: Returns the length of an array, slice, map, string, or channel.
*   `func make(t Type, size ...IntegerType) Type`: Creates a new slice, map, or channel.
*   `func new(Type) *Type`: Returns a pointer to a variable of the specified type, initialized with a zero value.
*   `func panic(v interface{})`: Stops the execution of the current `goroutine` and, if it's not intercepted, the program.
*   `func print(args ...Type)`: Writes the arguments to the standard error.
*   `func println(args ...Type)`: Writes the arguments to the standard error and adds a new line at the end.
*   `func real(c ComplexType) FloatType`: Returns the real part of a complex number.
*   `func recover() interface{}`: Stops a `panic` sequence and captures the `panic` value.

# Defer, Panic, and Recover

A very important keyword that hides a lot of complexity but makes it possible to execute many operations easily is `defer`. This is applied to a function, method, or closure execution and makes the function it precedes execute before the function returns. A common and very useful usage is closing resources. After opening the resource successfully, a deferred `close` statement will ensure that it is executed independently of the exit point, as shown in the following code:

```go
f, err := os.Open("config.txt")
if err != nil {
    return err
}
defer f.Close() // it will be closed anyways

// do operation on f
```

During a function's lifetime, all the deferred statements are added to a list and before exiting, they are executed in reverse order, from the last to the first `defer`.

These statements are executed even when there's a `panic`, which is why a deferred function with a `recover` call can be used to intercept a `panic` in the respective `goroutine` and avoid the `panic` that would kill the application otherwise. As well as a manual call to the `panic` function, there is a set of operations that will cause a `panic`, including the following:

*   Accessing a negative or non-existent array/slice index (index out of range)
*   Dividing an integer by `0`
*   Sending to a closed channel
*   Dereferencing on a `nil` pointer
*   Using a recursive function call that fills the stack (stack overflow)

`panic` should be used for errors that are not recoverable, which is why errors are just values in Go. Recovering a `panic` should be just an attempt to do something with that error before exiting the application. If an unexpected problem occurs, it's because it hasn't been handled correctly or some checks are missing. This represents a serious issue that needs to be dealt with, and the program needs to change, which is why it should be intercepted and dismissed.

# Concurrency Model

Concurrency is so central to Go that two of its fundamental tools are just keywords – `chan` and `go`. This is a very clever way of hiding the complexity of a well-designed and implemented concurrency model that is easy to use and understand.

## Understanding Channels and Goroutines

Channels are made for communicating, which is why Go's mantra is as follows:

> Do not communicate by sharing memory, share memory by communicating.

A channel is made for sharing data, and it usually connects two or more execution threads in an application, which makes it possible to send and receive data without worrying about data safety. Go has a lightweight implementation of a thread that is managed by the runtime instead of the operating system, and the best way to make them communicate is through the use of channels.

Creating a new `goroutine` is pretty easy – you just need to use the `go` operator, followed by a function execution. This includes method calls and closures. If the function has any arguments, they will be evaluated before the routine starts. Once it starts, there is no guarantee that changes to variables from an outer scope will be synchronized if you don't use channels:

```go
a := myType{}
go doSomething(a)     // function call
go func() {           // closure call
    // ...
}()                   // note that the closure is executed
go a.someMethod()     // method call
```

We already saw how to create a new channel with the `make` function. If a channel is unbuffered (`0` capacity), sending to the channel is a blocking operation that will wait for another `goroutine` to receive data from the same channel in order to unlock it. The capacity shows how many messages the channel is capable of holding before sending the next becomes a blocking operation:

```go
unbuf := make(chan int)    // unbuffered channel
buf := make(chan int, 3)   // channel with size 3
```

In order to send to a channel, we can use the `<-` operator. If the channel is on the left of the operator, it is a send operation, and if it's on the right, it is a receive operation. The value that's received from a channel can be assigned to a variable, as follows:

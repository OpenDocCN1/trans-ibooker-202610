This will be a specific type of `Result` that is either an `Ok` which always contains the unit type or some value that implements the `std::error::Error` trait:

```
type TestResult = Result<(), Box<dyn std::error::Error>>;
```

In the preceding code, `Box` indicates that the error will live inside a kind of pointer where the memory is dynamically allocated on the heap rather than the stack, and `dyn` “is used to highlight that calls to methods on the associated Trait are dynamically dispatched.” That’s really a lot of information, and I don’t blame you if your eyes glazed over. In short, I’m saying that the `Ok` part of `TestResult` will only ever hold the unit type, and the `Err` part can hold anything that implements the `std::error::Error` trait. These concepts are more thoroughly explained in *Programming Rust* (O'Reilly, 2021).
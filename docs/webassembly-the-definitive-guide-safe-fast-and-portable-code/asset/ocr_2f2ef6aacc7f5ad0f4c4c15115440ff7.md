on the other and one which depends on having a WASI capability of writing to the console.

Our *Cargo.toml* file shown in Example 12-11 has a few more dependencies than we have seen so far. The most important one, however, is the `wasmtime-wasi` dependency. This is an implementation of the standard WASI functionality that we are going to link against below.

# Example 12-11. A Cargo.toml file for our module linking example

```
[package]
name = "hello-modlink"
version = "0.1.0"
edition = "2018"

[dependencies]
wasmtime = "0.28.0"
wasmtime-wasi = "0.28.0"
anyhow = "1.0.19"
```

In the first module shown in Example 12-12, we import a function called `double` that will take an `i32`, double it, and return an `i32`. We are also importing a convenience function called `log` that will print a string in a Memory at the given offset and of the given length. We will also import a Memory instance to use and a global variable representing an offset to use as the location of our activity.

Our exported `run` function loads the constant “2” to the stack and then calls the `double` function. Remember that the top of the stack will contain the parameter so we expect this to produce the value “4”. We do not do anything with the output, but you can feel free to convince yourself that it does work. The point is mainly that our call to an imported function did actually work.

After doubling the value, we call the `log` function to print out “Hello, World!”. Notice we write our string into memory with a `data` element at the location specified by the global variable.

# Example 12-12. A module that we will link against that depends on another module
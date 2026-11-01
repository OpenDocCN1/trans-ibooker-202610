```
num_words,
num_bytes,
num_chars,
})
}
```

+   1. The `count` function will accept a mutable `file` value, and it might return a `FileInfo` struct.
+   2. Initialize mutable variables to count the lines, words, bytes, and characters.
+   3. For now, return a `FileInfo` with all zeros.

**NOTE**
I'm introducing the `impl` keyword to indicate that the `file` value must implement the `BufRead` trait. Recall that `open` returns a value that meets this criteria. You'll shortly see how this makes the function flexible.

In Chapter 3, I showed how to write a unit test, placing it just after the function it was testing. I'm going to create a unit test for the `count` function, but this time I'm going to place it inside a module called `tests`. This is a tidy way to group unit tests, and I can use a configuration option that tells Rust to only compile the module during testing. This is especially useful because I want to use `std::io::Cursor` in my test to make a fake filehandle for the `count` function. The module will not be included when I build and run the program when not testing.

A `Cursor` is “used with in-memory buffers, anything implementing `AsRef<[u8]>`, to allow them to implement `Read` and/or `Write`, allowing these buffers to be used anywhere you might use a reader or writer that does actual I/O.” Following is how I create the `tests` module and then import and test the `count` function:
std::io::stdin implement the BufRead trait, which means the values will, for instance, respond to the BufRead::lines function to produce lines of text. Note that BufRead::lines will remove any line endings such as \r\n on Windows and \n on Unix.

Again you see I'm using a Box to create a pointer to heap-allocated memory to hold the filehandle. You may wonder if this is completely necessary. I could try to write the function without using Box:

```
```rust
// This will not compile
fn open(filename: &str) -> MyResult<dyn BufRead> {
    match filename {
        "-" => Ok(BufReader::new(io::stdin())),
        _ => Ok(BufReader::new(File::open(filename)?)),
    }
}
```
```

If I try to compile this code, I get the following error:

```
```text
error[E0277]: the size for values of type `(dyn std::io::BufRead + 'static)`
cannot be known at compilation time
  --> src/lib.rs:88:28
   |
88 | fn open(filename: &str) -> MyResult<dyn BufRead> {
   |                         ^^^^^^^^^^^^^^^^^^^^^^^^
   |                         doesn't have a size known
   |                         at compile-time
   |
   = help: the trait `Sized` is not implemented for `(dyn std::io::BufRead + 'static)`
```
```

As the compiler says, there is not an implementation in BufRead for the Sized trait. If a variable doesn’t have a fixed, known size, then Rust can’t store it on the stack. The solution is to instead allocate memory on the heap by putting the return value into a Box, which is a pointer with a known size.
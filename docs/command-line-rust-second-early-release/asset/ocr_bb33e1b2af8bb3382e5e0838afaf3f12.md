+   3. Define a *public* (`pub`) function that returns either `Ok` containing the unit type `()` or some error `Err`.
+   4. Print *Hello, world!*.
+   5. Return an indication that the function ran successfully.

# TIP

By default, all the variables and functions in a module are private. In the preceding code, I must use `pub` to make this library function accessible to the rest of the program.

To call this, change `src/main.rs` to this:

```
```rust
fn main() {
    if let Err(e) = catr::run() { ①
        eprintln!("{}", e); ②
        std::process::exit(1); ③
    }
}
```
```

+   ① Execute the `catr::run` function and check if the return value matches an `Err(e)` where `e` is some value that implements the `Error` trait, which means among other things that it can be printed.
+   ② Use the `eprintln!` (*error print line*) macro to print the error message `e` to `STDERR`.
+   ③ Exit the program with a nonzero value to indicate an error.
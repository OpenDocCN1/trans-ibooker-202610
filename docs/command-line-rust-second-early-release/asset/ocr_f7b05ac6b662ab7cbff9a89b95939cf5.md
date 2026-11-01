+   - How to use an external Rust crate from crates.io
+   - About the exit status of a program
+   - How to use common systems commands and options
+   - How write Rust versions of the `true` and `false` programs
+   - How to organize, write, and run tests

# Getting Started with “Hello, world!”

The place to start is at the beginning, and it seems universally agreed that this means printing “Hello, world!” to the screen. You might change to a temporary directory with `cd /tmp` to write this first program, then fire up a text editor and type this Rust program into a file called *hello.rs*:

```
fn main() { ①
    println!("Hello, world!"); ②
} ③
```

① Functions are defined using `fn`. The name of this function is `main`.

② `println!` (*print line*) is used to print to `STDOUT` (pronounced *standard out*). The semicolon indicates the end of the statement.

③ The body of the function is enclosed in curly braces.

Rust will automatically start in the `main` function. Functions arguments appear inside the parentheses that follow. Because there are no arguments listed in `main()`, the function takes no arguments. The last thing I’ll point out here is `println!` looks like a function but is actually a `macro`, which is essentially code that writes code. You
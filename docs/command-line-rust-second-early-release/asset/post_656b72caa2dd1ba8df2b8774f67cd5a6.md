
```
#[test]
fn false_not_ok() {
    let mut cmd = Command::cargo_bin("false").unwrap();
    cmd.assert().failure(); ①
}
```

① Use the Assert::failure function to “ensure the command failed.”

Run cargo test to verify that the programs all work as expected:

```
running 3 tests
test runs ... ok
test true_ok ... ok
test false_not_ok ... ok
```

Another way to write this program uses std::process::abort:

```
$ cat src/bin/false.rs
fn main() {
    std::process::abort();
}
```

Again, run the test suite to ensure that the program still works as expected.

## Testing the Program Output

While it’s nice to know that my hello program exits correctly, I’d like to ensure it actually prints the correct output to STDOUT (pronounced standard out), which is the standard place for output to appear and is usually the console. Update your runs function in tests/cli.rs to the following:

```
#[test]
fn runs() {
    let mut cmd = Command::cargo_bin("hello").unwrap();
    cmd.assert().success().stdout("Hello, world!\n");
}
```

① Verify that the command exits successfully and prints the given text to STDOUT.

Run the tests and verify that hello does, indeed, work correctly. Next, change src/main.rs to add some more exclamation points:

```
$ cat src/main.rs
fn main() {
    println!("Hello, world!!!");
}
```

Run the tests again to observe a failing test:

```
running 3 tests
test true_ok ... ok
test false_not_ok ... ok
test runs ... FAILED

failures:

---- runs stdout ----
thread runs panicked at 'Unexpected stdout, failed diff var original
|__ original: Hello, world!
|__ diff:
--- value     expected
+ value actual
@@ -1 +1 @@
-Hello, world!
+Hello, world!!!
|__ var as str: Hello, world!!!
```

Learning to read test output is a skill in itself, but the output is trying very hard to show you what was expected output and what was the *actual* output. While this is a trivial program, I hope you can see the value in automatically checking all aspects of the program we write.

## Exit Values Make Programs Composable

Correctly reporting the exit status is a characteristic of well-behaved systems programs. The exit value is important because a failed process used in conjunction with another process should cause the combination to fail. For instance, I can use the logical *and* operator && in bash to chain the two commands true and ls. Only if the first process reports success will the second process run:

```
$ true && ls
Cargo.lock  Cargo.toml  src/        target/     tests/
```

If instead I execute false && ls, the result is that the first process fails and ls is never executed. Additionally, the exit status of the whole command is nonzero.

```
$ false && ls
$ echo $?
1
```

Systems programs that correctly report errors make them composable with other programs. This is important because it’s extremely common in Unix environments to combine many small commands to make *ad hoc* programs on the command line. If a program encounters an error but fails to report it to the operating system, then the results could be incorrect. It’s far better for a program to abort so that the underlying problems can be fixed.

## Summary

This chapter was meant to introduce you to some key ideas about organizing a Rust project and some basic ideas about systems programs. Here are some of the things you should understand:

- The Rust compiler `rustc` can compile Rust source code into a machine-executable file on Windows, macOS, and Linux.
- The Cargo tool can help create a Rust project. You can also use it to compile, run, and test the code.
- You saw several examples of systems tools like `ls`, `cd`, `mkdir`, and `rm` that accept arguments like file or directory name as well as options like `-f` or `-p`.
- POSIX-compatible programs should exit with a value of `0` to indicate success and any value 1-255 to indicate an error.
- You wrote the `hello` program to say “Hello, world!” and saw how to test it for the exit status and the text it printed to `STDOUT`.
- You learned to add crate dependencies to *Cargo.toml* and use the crates in your code.
- You created a *tests* directory to organize testing code, and you used `#[test]` to make functions that should be as tests.
- You learned how to write, run, and test alternate binaries in a Cargo project by creating source code files in the *src/bin* directory.
- You wrote your own implementations of the `true` and `false` programs along with tests to verify that they succeed and fail as expected. You saw that by default a Rust program will exit with the value `0` and that the `std::process::exit` function can be used to explicitly exit with a given code. Additionally, the `std::process::abort` function can be used to exit with an error code.

1. TOML stands for Tom’s Obvious, Minimal Language.
2. Shell aliases and functions can also be executed like commands, but I’m only talking about finding programs to run at this point.

## Chapter 2. Test for Echo

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 2nd Chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at kyClark@gmail.com.

> *By the time you get this note, we’ll no longer be alive, we’ll have all gone up in smoke, there’ll be no way to reply.*
> 
> —They Might Be Giants

In Chapter 1, you wrote programs that always behave the same way. In this chapter, you will learn how to use arguments from the user to change the behavior of the program at runtime. The challenge program you’ll write in this chapter is a clone of `echo`, which will print its arguments on the command line, optionally terminated with a newline.

In this chapter, you’ll learn:

- How to process command-line arguments with the `clap` crate
- About `STDOUT` and `STDERR`
- About Rust types like strings, vectors, slices, and the unit type
- How to use expressions like `match`, `if`, and `return`
- How to use `Option` to represent an optional value
- How to handle errors using the `Result` variants of `Ok` and `Err`
- How to exit a program and signal success or failure
- The difference between stack and heap memory

## Starting a New Binary Program with Cargo

This challenge program will be called `echor` for `echo` plus `r` for Rust. (I can’t decide if I pronounce this like eh-core or eh-koh-ar.) I’m hoping you have created a directory to hold all the projects you’ll write in this book. Change into that directory and use Cargo to start your *crate*, which is a Rust binary program or library:

```
$ cargo new echor
    Created binary (application) `echor` package
```

You should see a familiar directory structure:

```
$ tree echor/
echor/
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

Go into the new *echor* directory and use Cargo to run the program:

```
$ cd echor
$ cargo run
```

Hello, world! ①

① The default program always prints “Hello, world!”

You’ve already seen this program in Chapter 1, but I’d like to point out a couple more things about the code:

```
$ cat src/main.rs
fn main() {
    println!("Hello, world!");
}
```

As you saw in Chapter 1, Rust will start the program by executing the main function in src/main.rs. Any arguments to the function are contained in the parentheses after the function name, so the empty parentheses here indicate the function takes no arguments. All functions return a value, and the return type may be indicated with an arrow and the type such as -> u32 to say the function returns an unsigned 32-bit integer. The lack of any return type for main implies that the function returns what Rust calls the unit type. Note that the println! macro will automatically append a newline to the output, which is a feature I’ll need to control when the user requests no terminating newline.

## NOTE

The unit type is like an empty value and is signified with a set of empty parentheses (). The documentation says this “is used when there is no other meaningful value that could be returned.” It’s not quite like a null pointer or undefined value in other languages, a concept first introduced by Tony Hoare (no relation to Rust creator Graydon Hoare) who called the null reference his “billion-dollar mistake.” Since Rust does not (normally) allow you to dereference a null pointer, it must logically be worth at least a billion dollars.

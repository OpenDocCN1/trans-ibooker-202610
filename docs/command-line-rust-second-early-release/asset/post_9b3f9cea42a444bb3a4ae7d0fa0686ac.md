
```
echor [FLAGS] <TEXT>...

For more information try --help
```

This was an error, and so you can inspect the exit value to verify that it’s not 0:

```
$ echo $?
1
```

If you try to provide any argument that isn’t defined, it will trigger an error and a nonzero exit value:

```
$ cargo run -- -x
error: Found argument '-x' which wasn't expected, or isn't valid in this context

USAGE:
    echor [FLAGS] <TEXT>...

For more information try --help
```

## NOTE

You might wonder how this magical stuff is happening. Why is the program stopping and reporting these errors? If you read the documentation for App::get_matches, you’ll see that “upon a failed parse an error will be displayed to the user and the process will exit with the appropriate error code.”

There’s another subtle thing happening with the output that is not at all obvious. The usage and error messages are all appearing on STDERR (pronounced standard error), which is another channel of Unix output. To see this in the bash shell, I can redirect channel 1 (STDOUT) to a file called out and channel 2 (STDERR) to a file called err.

```
$ cargo run 1>out 2>err
```

You should see no results from that command because all the output was redirected to files. The out file should be empty because there was nothing printed to STDOUT, but the err file should contain the output from Cargo and the error messages from the program:

```
$ cat err
    Finished dev [unoptimized + debuginfo] target(s) in
0.01s
    Running `target/debug/echor`
error: The following required arguments were not provided:
    <TEXT>...

USAGE:
    echor [FLAGS] <TEXT>...

For more information try --help
```

So you see that another hallmark of well-behaved systems programs is to print regular output to STDOUT and error messages to STDERR. Sometimes errors are severe enough that you should halt the program, but sometimes they should just be noted in the course of running. For instance, in Chapter 3 you will write a program that processes input files, some of which will intentionally not exist or will be unreadable. I will show you how to print warnings to STDERR about these files and skip to the next argument.

## Creating the Program Output

My next step is to use the values provided by the user to create the program’s output. It’s common to copy the values out of the matches into variables. To start, I want to extract the text argument. Because this Arg was defined to accept one or more values, I can use either of these functions that return multiple values:

- ArgMatches::values_of: returns Option<Values>
- `ArgMatches::values_of_lossy`: returns `Option<Vec<String>>`

To decide which to use, I have to run down a few rabbit holes to understand the following concepts:

- `Option`: a value that is either `None` or `Some(T)` where T is any type like a string or an integer. In the case of `ArgMatches::values_of_lossy`, the type T will be a vector of strings.
- `Values`: An iterator for getting multiple values out of an argument.
- `Vec`: A vector, which is a contiguous growable array type.
- `String`: A string of characters.

Both of the functions `ArgMatches::values_of` and `ArgMatches::values_of_lossy` will return an `Option` of something. Since I ultimately want to print the strings, I will use `ArgMatches::values_of_lossy` function to get an `Option<Vec<String>>`. The `Option::unwrap` function will take the value out of `Some(T)` to get at the payload T. Because the text argument is required by `clap`, I know it will be impossible to have `None`; therefore, I can safely call `Option::unwrap` to get the `Vec<String>` value:

```
let text = matches.values_of_lossy("text").unwrap();
```

## WARNING

If you call `Option::unwrap` on a `None`, it will cause a `panic` that will crash your program. You should only call `unwrap` if you are positive the value is the `Some` variant.

The `omit_newline` argument is a bit easier as it's either present or not. The type of this value will be a `bool` or Boolean, which is either `true` or `false`:

```
let omit_newline = matches.is_present("omit_newline");
```

Finally, I want to print the values. Because `text` is a vector of strings, I can use `Vec::join` to join all the strings on a single space into a new string to print. Inside the `echor` program, `clap` will be creating the vector. To demonstrate how `Vec::join` works, I'll show you how create a vector using the `vec!` macro:

```
let text = vec!["Hello", "world"];
```

## NOTE

The values in Rust vectors must all be of the same type. Dynamic languages often allow lists to mix types like strings and numbers, but Rust will complain about “mismatched types.” Here I want a list of literal strings which must be enclosed in double quotes. The `str` type in Rust represents a valid UTF-8 string. I'll have more to say about UTF in Chapter 4.

`Vec::join` will insert the given string between all the elements of the vector to create a new string. I can use `println!` to print the new string to `STDOUT` followed by a newline:

```
println!("{} ", text.join(" "));
```

It's common practice in Rust documentation to present facts using `assert!` to say that something is `true` or `assert_eq!` to demonstrate that one thing is equivalent to another. In the following code, I can assert that the result of `text.join(" ")` is equal to the string "Hello world":

```
assert_eq!(text.join(" "), "Hello world");
```

When the -n flag is present, the output should omit the newline. I will instead use the print! macro which does not add a newline, and I will choose to add either a newline or the empty string depending on the value of omit_newline. Depending on your background, you might try to write something like this:

```
let ending = "\n"; ①
if omit_newline {
    ending = ""; // This will not work ②
}
print!("{}{}", text.join(" "), ending); ③
```

- ① Assign a default value.
- ② Change the value if the newline should be omitted.
- ③ Use print! which will not add a newline to the output.

If I try to run this code, Rust tells me that I cannot reassign the value of ending:

```
$ cargo run -- Hello world
error[E0384]: cannot assign twice to immutable variable `ending`
  --> src/main.rs:27:9
  |
25 |     let ending = "\n";
  |           ------
  |           |
  |           first assignment to `ending`
  |           help: make this binding mutable: `mut ending`
26 |     if omit_newline {
27 |         ending = ""; // This will not work
  |         ^^^^^^^^^^^ cannot assign twice to immutable variable
```

Something that really sets Rust apart from other languages is that variables are *immutable* by default, meaning they can’t be altered from their initial value. I’m not allowed to reassign the value of the variable ending; however, the compiler tells me to add mut to make the variable *mutable*:

```
let mut ending = "\n";
if omit_newline {
    ending = "";
}
print!("{}{}", text.join(" "), ending);
```

① Add mut to make this a mutable value.

There’s a much better way to write this. In Rust, if is an expression and not a statement. An *expression* returns a value, but a statement does not. Here’s a more Rustic way to write this:

```
let ending = if omit_newline { "" } else { "\n" };
```

## NOTE

> An if without an else will return the unit type. The same is true for a function without a return type, so the main function returns ().

Since I only use ending in one place, I don’t need to assign it to a variable. Here is how I would update the main function:

```
fn main() {
    let matches = ...; // Same as before
    let text = matches.values_of_lossy("text").unwrap();
    let omit_newline = matches.is_present("omit_newline");
    print!("{}{}", text.join(" "), if omit_newline { "" } else { "\n" });
}
```

With these changes, the program appears to work correctly; however, I’m not willing to stake my reputation on this. I need to, as the Russian saying goes, “Доверяй, но проверяй.”¹ This requires that I write some tests to run my program with various inputs and verify that it produces the same output as the original echo program.

## Integration and Unit Tests

In Chapter 1, I showed how to create integration tests that run the program from the command line just as the user will do to ensure it works correctly. In this chapter, I’ll also show you how to write unit tests that exercise individual functions, which might be considered a unit of programming. To get started, you should add the following dependencies to Cargo.toml. Note that I’m adding predicates to this project:

```toml
[dev-dependencies]
assert_cmd = "1"
predicates = "1"
```

I often write tests that ensure my programs fail when run incorrectly. For instance, this program ought to fail and print help documentation when provided no arguments. Create a tests directory, and then create tests/cli.rs with the following:

```rust
use assert_cmd::Command;
use predicates::prelude::*; ①

#[test]
fn dies_no_args() {
    let mut cmd = Command::cargo_bin("echor").unwrap();
    cmd.assert() ②
        .failure()
        .stderr(predicate::str::contains("USAGE"));
}
```

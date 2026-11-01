
The next test passes two arguments, Hello and there, and expects the program to print Hello there.

```
#[test]
fn hello2() -> TestResult {
    let expected =
fs::read_to_string("tests/expected/hello2.txt")?;
    let mut cmd = Command::cargo_bin("echor")?;
    cmd.args(vec!["Hello", "there"]) ①
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```

① Use the Command::args method to pass a vector of arguments rather than a single string value.

I have a total of four files to check, so it behooves me to write a helper function. I'll call it run and will pass it the argument strings along with the expected output file. Rather than use a vector for the arguments, I'm going to use a std::slice because I don't need to grow the argument list after I've defined it:

```
fn run(args: &[&str], expected_file: &str) -> TestResult {
    let expected = fs::read_to_string(expected_file)?; ②
    Command::cargo_bin("echor")? ③
        .args(args)
        .assert()
        .success()
        .stdout(expected);
    Ok(()) ④
}
```

① The args will be a slice of &str values, and the expected_file will be a &str. The return value is a TestResult.

- 2. Try to read the contents of the `expected_file` into a string.
- 3. Attempt to run `echor` in the current crate with the given arguments and assert that `STDOUT` is the expected value.
- 4. If all the previous code worked, return `Ok` containing the unit type.

## NOTE

You will find that Rust has many types of “string” variables. The type `str` is appropriate here for literal strings in the source code. The & shows that I intend only to borrow the string for a little while. I'll have more to say about strings, borrowing, and ownership later.

Below is how I can use the helper function to run all four tests. Replace the earlier `hello1` and `hello2` definitions with these:

```
#[test]
fn hello1() -> TestResult {
    run(&["Hello there"], "tests/expected/hello1.txt") ①
}

#[test]
fn hello2() -> TestResult {
    run(&["Hello", "there"], "tests/expected/hello2.txt")
②
}

#[test]
fn hello1_no_newline() -> TestResult {
    run(&["Hello there", "-n"],
"tests/expected/hello1.n.txt") ③
}

#[test]
fn hello2_no_newline() -> TestResult {
    run(&["-n", "Hello", "there"],
"tests/expected/hello2.n.txt") ④
}
```

- 1. A single string value as input.
- 2. Two strings as input.
- 3. A single string value as input with `-n` flag to omit the newline. Note that there are two spaces between the words.
- 4. Two strings as input with `-n` flag appearing first.

As you can see, I can write as many functions as I like in *tests/cli.rs*. Only those marked with `#[test]` are run when testing. If you run **cargo test** now, you should see five passing tests (in no particular order):

```
running 5 tests
test dies_no_args ... ok
test hello1 ... ok
test hello1_no_newline ... ok
test hello2_no_newline ... ok
test hello2 ... ok
```

## Summary

Now you have written about 30 lines of Rust code in *src/main.rs* for the `echor` program and five tests in *tests/cli.rs* to verify that your program meets some measure of specification (the *specs*, as they say). Consider what you’ve achieved:

- Well-behaved systems programs should print basic output to `STDOUT` and errors to `STDERR`.
- You’ve written a program that takes the options `-h|--help` to produce help, `-V|--version` to show the program’s version, and `-n` to omit a newline along with one or more positional command line arguments.
- If the program is run with the wrong arguments or with the `-h|--help` flag, it will print usage documentation.
- The program will echo back all the command line arguments joined on spaces.
- The trailing newline will be omitted if the `-n` flag is present.
- You can run integration tests to confirm that your program replicates the output from `echo` for at least four test cases covering one or two inputs both with and without the trailing newline.
- You learned to use several Rust types including the unit type, strings, vectors, slices, `Option`, and `Result` as well as how to create type alias to a specific type of `Result` called a `TestResult`.
- You used a `Box` to create a smart pointer to heap memory. This required digging a bit into the differences between the stack—where variables have a fixed, known size and are accessed in LIFO order—and the heap—where the size of variables may change during the program and are accessed through a pointer.
- You learned how to read the entire contents of a file into a string.
- You learned how to execute an external command from within a Rust program, check the exit status, and verify the contents of both `STDOUT` and `STDERR`.

All this, and you've done it while writing in a language that simply will not allow you to make common mistakes that lead to buggy programs or security vulnerabilities. Feel free to give yourself a little high five or enjoy a slightly evil MWUHAHA chuckle as you consider how Rust will help you conquer the world. Now that I've shown how to organize and write tests and data, I’ll use the tests earlier in the next program so I can start using *test-driven development* where I write tests first then write code to satisfy the tests.

1 “Trust, but verify.” Apparently this rhymes in Russian and so sounds cooler than when Reagan used it in the 1980s during nuclear disarmament talks with the USSR.

## Chapter 3. On The Catwalk

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 3rd Chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at kyclark@gmail.com.

> *When you are alone, you are the cat, you are the phone. You are an animal.*
> 
> —They Might Be Giants

In this chapter, the challenge is to write a clone of the `cat` program, which is so named because it can *concatenate* many files into one file. That is, given files *a*, *b*, and *c*, you could execute `cat a b c > all` to stream all the lines from these three files and redirect them into a file called *all*. The program will accept an option to prefix each line with the line number.

In this chapter, you’ll learn:

- How to organize your code into a library and a binary crate
- How to use testing-first development
- The difference between public and private variables and functions
- How to test for the existence of a file
- How to create a random string for a file that does not exist
- How to read regular files or `STDIN` (pronounced *standard in*)
- How to use `eprintln!` to print to `STDERR` and `format!` to format a string
- How to write a test that provides input on `STDIN`
- How and why to create a `struct`
- How to define mutually exclusive arguments
- How to use the `enumerate` method of an iterator
- More about how and why to use a `Box`

## How cat Works

I’ll start by showing how cat works so that you know what is expected of the challenge. The BSD version of cat does not respond to --help, so I must use man cat to read the manual page. For such a limited program, it has a surprising number of options:

```
CAT(1)                              BSD General Commands Manual
CAT(1)

NAME
    cat -- concatenate and print files

SYNOPSIS
    cat [-benstuv] [file ...]

DESCRIPTION
```

The cat utility reads files sequentially, writing them to the standard output. The file operands are processed in command-line order. If file is a single dash ('-') or absent, cat reads from the standard input. If file is a UNIX domain socket, cat connects to it and then reads it until EOF. This complements the UNIX domain binding capability available in inetd(8).

The options are as follows:

- **-b** Number the non-blank output lines, starting at 1.
- **-e** Display non-printing characters (see the -v option), and display a dollar sign ('$') at the end of each line.
- **-n** Number the output lines, starting at 1.
- **-s** Squeeze multiple adjacent empty lines, causing the output to be single spaced.
- **-t** Display non-printing characters (see the -v option), and display tab characters as '^I'.
- **-u** Disable output buffering.
- **-v** Display non-printing characters so they are visible. Control characters print as '^X' for control-X; the delete character (octal 0177) prints as '^*?'. Non-ASCII characters (with the high bit set) are printed as 'M-' (for meta) followed by the character for the low 7 bits.

## EXIT STATUS

+   2. Try to read the contents of the `expected_file` into a string.
+   3. Attempt to run `echor` in the current crate with the given arguments and assert that `STDOUT` is the expected value.
+   4. If all the previous code worked, return `Ok` containing the unit type.

# NOTE

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
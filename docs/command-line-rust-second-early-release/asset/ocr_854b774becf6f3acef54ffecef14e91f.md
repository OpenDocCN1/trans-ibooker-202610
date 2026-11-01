If I ask head to select just the first byte from this file, I get the byte value 195, which is not a valid UTF-8 string. The output is a special character that indicates a problem converting a character into Unicode:

```
$ head -c 1 tests/inputs/one.txt
?
```

The challenge program is expected to recreate this behavior. This is a challenging program to write, but you should be able to use std::io, std::fs::File, and std::io::BufReader to figure out how to read bytes and lines from each of the files. I've included a full set of tests in tests/cli.rs that you should have copied into your source tree. Be sure to run cargo test frequently to check your progress. Do your best to pass all the tests before looking at my solution.

# Solution

I was really surprised by how much I learned by writing this program. What I expected to be a rather simple program proved to be very challenging. I'd like to step you through how I arrived at my solution, starting with how I read a file line-by-line.

# Reading a File Line-by-line

To start, I will modify some code from Chapter 3 for reading the lines from a file:

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                for line in file.lines().take(config.lines)
{ ①
```
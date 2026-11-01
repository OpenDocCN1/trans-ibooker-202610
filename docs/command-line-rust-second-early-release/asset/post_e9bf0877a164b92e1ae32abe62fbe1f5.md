
(i.e. force
    grep to behave as egrep).

-e pattern, --regexp=pattern
    Specify a pattern used during the search of the
input: an input
    line is selected if it matches any of the specified
patterns.
    This option is most useful when multiple -e options
are used to
    specify multiple patterns, or when a pattern begins
with a dash
    (`-').

The converse of these options is:

-G, --basic-regexp
    Interpret pattern as a basic regular expression
(i.e. force grep
    to behave as traditional grep).

Regular expressions have been around since the 1950s when they were invented by the American mathematician Stephen Cole Kleene². Since that time, the syntax has been modified and expanded by various groups, perhaps most notably by the Perl community which created Perl Compatible Regular Expressions (PCRE). By default, grep will only parse basic regexes, but the preceding flags can allow it to use other varieties. For instance, I can use the pattern ee to search for any lines containing two adjacent es:

```
$ grep 'ee' tests/inputs/*
tests/inputs/bustle.txt:The sweeping up the heart,
```

If I wanted to find any character that is repeated twice, the pattern is (.)\1 where the dot (.) represents any character and the capturing parentheses allow me to use the backreference \1 to refer to the
first capture group. This is an example of an extended expression, and so requires the -E flag:

```
$ grep -E '(.)\1' tests/inputs/*
tests/inputs/bustle.txt:The sweeping up the heart,
tests/inputs/bustle.txt:And putting love away
tests/inputs/bustle.txt:We shall not want to use again
tests/inputs/nobody.txt:Are you–Nobody–too?
tests/inputs/nobody.txt:Don't tell! they'd advertise–you know!
tests/inputs/nobody.txt:To tell one's name–the livelong June–
```

The Rust `regex` crate documentation notes that its “syntax is similar to Perl-style regular expressions, but lacks a few features like look around and backreferences.” (*Look-around assertions* allow the expression to assert that a pattern must be followed or preceded by another pattern, and *backreferences* allow the pattern to refer to previously captured values.) This means that the challenge program will work more like the `egrep` in handling extended regular expressions by default. Sadly, this also means that the program will not be able to handle the preceding pattern because it requires backreferences. It will still be a wicked cool program to write, so let’s get at it.

## Finding the Files to Search

To use the compiled regex, I next need to find all the files to search. Recall that the user might provide directory names with the -- `recursive` option to search for all the files contained in each directory; otherwise, directory names should result in a warning printed to `STDERR`. I decided to write a function called `find_files` that will accept a vector of strings which may be file or directory names along with a Boolean for whether or not to recurse into directories. It returns a vector of `MyResult` values that will either hold a string which is the name of a valid file or an error message:

```
fn find_files(files: &[String], recursive: bool) ->
Vec<MyResult<String>> {
    unimplemented!();
}
```

To test this, I can add a `tests` module to `src/lib.rs`. Note that this will use the `rand` module which should be listed in the `[dev-dependencies]` section of your *Cargo.toml* as noted earlier in the chapter:

```
#[cfg(test)]
mod tests {
    use super::find_files;
    use rand::{distributions::Alphanumeric, Rng};

    #[test]
    fn test_find_files() {
        // Verify that the function finds a file known to exist
        let files =
            find_files(&
["./tests/inputs/fox.txt".to_string()], false);
        assert_eq!(files.len(), 1);
        assert_eq!(files[0].as_ref().unwrap(),
"./tests/inputs/fox.txt");

        // The function should reject a directory without
the recursive option
        let files = find_files(&
["./tests/inputs".to_string()], false);
        assert_eq!(files.len(), 1);
        if let Err(e) = &files[0] {
            assert_eq!(
                e.to_string(),
                "./tests/inputs is a directory".to_string()
            );
        }

        // Verify the function recurses to find four files
in the directory
        let res = find_files(&
["./tests/inputs".to_string()], true);
        let mut files: Vec<String> = res
```

```
.iter()
.map(|r| r.as_ref().unwrap().replace("\\", "/"))
.collect();
files.sort();
assert_eq!(files.len(), 4);
assert_eq!(
    files,
    vec![
        "./tests/inputs/bustle.txt",
        "./tests/inputs/empty.txt",
        "./tests/inputs/fox.txt",
        "./tests/inputs/nobody.txt",
    ]
);

// Generate a random string to represent a
nonexistent file
    let bad: String = rand::thread_rng()
        .sample_iter(&Alphanumeric)
        .take(7)
        .map(char::from)
        .collect();

// Verify that the function returns the bad file as
an error
    let files = find_files(&[bad], false);
    assert_eq!(files.len(), 1);
    assert!(files[0].is_err());
}
}
```

You should be able to run cargo test test_find_files to verify that your function finds existing files, recurses properly, and will report nonexistent files as errors. Here is how I can use it in my code:

```
pub fn run(config: Config) -> MyResult<()> {
    println!("pattern \"{}\"", config.pattern);

    for entry in find_files(&config.files,
config.recursive) {
        match entry {
```

```
Err(e) => eprintln!("{}", e),
Ok(filename) => println!("file \"{}\"",
filename),
}
}

Ok(())
}
```

My solution uses WalkDir, which I introduced in Chapter 7 (findr).
See if you can get your program to reproduce the following output.
To start, the default input should be "-" to represent reading from STDIN:

```
$ cargo run -- fox
pattern "fox"
file "-"
```

## NOTE

Printing a regular expression means calling the Regex::as_str method.
Regex::build notes that this “will produce the pattern given to new verbatim.
Notably, it will not incorporate any of the flags set on this builder.”

Explicitly listing "-" as the input should produce the same output:

```
$ cargo run -- fox -
pattern "fox"
file "-"
```

The program should handle multiple input files:

```
$ cargo run -- fox tests/inputs/*
pattern "fox"
file "tests/inputs/bustle.txt"
file "tests/inputs/empty.txt"
file "tests/inputs/fox.txt"
file "tests/inputs/nobody.txt"
```

A directory name without a --recursive option should be rejected:

```
$ cargo run -- fox tests/inputs
pattern "fox"
tests/inputs is a directory
```

With the --recursive flag, it should find the directory’s files:

```
$ cargo run -- -r fox tests/inputs
pattern "fox"
file "tests/inputs/empty.txt"
file "tests/inputs/nobody.txt"
file "tests/inputs/bustle.txt"
file "tests/inputs/fox.txt"
```

Nonexistent arguments should be printed to STDERR in the course of handling each entry:

```
$ cargo run -- -r fox blargh tests/inputs/fox.txt
pattern "fox"
blargh: No such file or directory (os error 2)
file "tests/inputs/fox.txt"
```

## Finding the Matching Lines of Input

Once you are properly handling the inputs, it’s time to open the files and search for matching lines. I suggest you again use the open function from earlier chapters that will open and read either an existing file or STDIN for the filename "-". You will need to add use std::fs::File and use std::io::{self, BufRead, BufReader} for this:

```
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ =>
Ok(Box::new(BufReader::new(File::open(filename)?))),
    }
}
```

When reading the lines, be sure to preserve the line endings as one of the input files contains Windows-style CRLF endings. My solution uses a function called `find_lines` which you can start with the following:

```
fn find_lines<T: BufRead>(
    mut file: T, ①
    pattern: &Regex, ②
    invert_match: bool, ③
) -> MyResult<Vec<String>> {
    unimplemented!();
}
```

- ① The `file` must implement the `std::io::BufRead` trait.
- ② The `pattern` is a reference to a compiled regular expression.
- ③ The `invert_match` is a Boolean for whether to reverse the match operation.

NOTE

In Chapter 5, I used `impl BufRead` to indicate a value that must implement the `BufRead`. In the preceding code, I'm using `<T: BufRead>` to write the trait bound for the type T.

To test this function, I expanded my `tests` module by adding the following `test_find_lines` function which again uses `std::io::Cursor` to create a fake filehandle that implements `BufRead` for testing:

```
#[cfg(test)]
mod test {
    use super::{find_files, find_lines};
    use rand::{distributions::Alphanumeric, Rng};
    use regex::{Regex, RegexBuilder};
```

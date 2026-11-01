
Normally, the positional arguments are files, and the inclusion of a directory such as my $HOME directory will cause grep to print a warning:

```
$ grep The bustle.txt $HOME fox.txt
bustle.txt:The bustle in a house
bustle.txt:The morning after death
bustle.txt:The sweeping up the heart,
grep: /Users/kyclark: Is a directory
fox.txt:The quick brown fox jumps over the lazy dog.
```

Directory names are only acceptable when using the -r|--recursive option to find all the files in a directory that contain matching text:

```
$ grep -r The .
./nobody.txt:Then there's a pair of us!
./bustle.txt:The bustle in a house
./bustle.txt:The morning after death
./bustle.txt:The sweeping up the heart,
./fox.txt:The quick brown fox jumps over the lazy dog.
```

The -r and -i short flags can be combined to perform a recursive, case-insensitive search of one or more directories:

```
$ grep -ri the .
./nobody.txt:Then there's a pair of us!
./nobody.txt:Don't tell! they'd advertise-you know!
./nobody.txt:To tell one's name-the livelong June-
./bustle.txt:The bustle in a house
./bustle.txt:The morning after death
./bustle.txt:The sweeping up the heart,
./fox.txt:The quick brown fox jumps over the lazy dog.
```

Without any positional arguments for inputs, grep will read STDIN:

```
$ cat * | grep -i the
The bustle in a house
The morning after death
The sweeping up the heart,
The quick brown fox jumps over the lazy dog.
Then there's a pair of us!
Don't tell! they'd advertise-you know!
To tell one's name-the livelong June-
```

This is as far as the challenge program is expected to go.

## Getting Started

The name of the challenge program should be `grepr` for a Rust version of `grep`. Start with `cargo new grepr`, then copy the *09_grepr/tests* directory into your new project. In addition to using `clap` to parse the command-line arguments, my solution will use `regex` for regular expressions and `walkdir` to find the input files. Here is how I start the *Cargo.toml*:

```
[dependencies]
clap = "2.33"
regex = "1"
walkdir = "2"
sys-info = "0.9"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

You can run `cargo test` to perform an initial build and run the tests, all of which should fail.

## Defining the Arguments

I will start with the following for *src/main.rs*:

```
fn main() {
    if let Err(e) = grepr::get_args().and_then(grepr::run)
{
        eprintln!("{}", e);
        std::process::exit(1);
    }
}
```

Following is how I started my src/lib.rs. Note that all the Boolean options default to false:

```
use clap::{App, Arg};
use regex::{Regex, RegexBuilder};
use std::error::Error;

type MyResult<T> = Result<T, Box<dyn Error>>;

#[derive(Debug)]
pub struct Config {
    pattern: Regex, ①
    files: Vec<String>, ②
    recursive: bool, ③
    count: bool, ④
    invert_match: bool, ⑤
}
```

① The pattern is a compiled regular expression.

② The files is a vector of strings.

③ The recursive option is a Boolean to recursively search directories.

④ The count option is a Boolean to display a count of the matches.

⑤ The invert_match is a Boolean to find lines that do not match the pattern.

Here is how I started my get_args and run functions. You should fill in the missing parts:

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("grepr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust grep")
        // What goes here?
        .get_matches();

    Ok(Config {
        pattern: ...
        files: ...
        recursive: ...
        count: ...
        invert_match: ...
    })
}

pub fn run(config: Config) -> MyResult<()> {
    println!("{:#?}", config);
    Ok(())
}
```

Your program should be able to produce the following usage:

```
grepr 0.1.0
Ken Youens-Clark <kyclark@gmail.com>
Rust grep

USAGE:
    grepr [FLAGS] <PATTERN> <FILE>...

FLAGS:
    -c, --count          Count occurrences
    -h, --help           Prints help information
    -i, --insensitive    Case-insensitive
    -v, --invert-match   Invert match
    -r, --recursive      Recursive search
    -V, --version        Prints version information

ARGS:
    <PATTERN>    Search pattern ①
    <FILE>...    Input file(s) [default: -] ②
```

① The search pattern is a required argument.

② The input files are optional and default to “-” for STDIN.

Your program should be able to print a Config like the following when provided a pattern and no input files:

```
$ cargo run -- dog
Config {
    pattern: dog,
    files: [
        "-",
    ],
    recursive: false,
    count: false,
    invert_match: false,
}
```

It should be able to handle one or more input files and handle the flags:

```
$ cargo run -- dog -ricv tests/inputs/*.txt
Config {
    pattern: dog,
    files: [
        "tests/inputs/bustle.txt",
        "tests/inputs/empty.txt",
        "tests/inputs/fox.txt",
        "tests/inputs/nobody.txt",
    ],
    recursive: true,
    count: true,
    invert_match: true,
}
```

It should reject an invalid regular expression. For instance, * signifies zero or more of the preceding pattern. By itself, this is incomplete:

```
$ cargo run -- \*
Invalid pattern "*"
```

I assume you figured that out. Following is how I declared my arguments:

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("grepr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust grep")
        .arg(
            Arg::with_name("pattern") ①
                .value_name("PATTERN")
                .help("Search pattern")
                .required(true),
        )
        .arg(
            Arg::with_name("files") ②
                .value_name("FILE")
                .help("Input file(s)")
                .required(true)
                .default_value("-")
                .min_values(1),
        )
        .arg(
            Arg::with_name("insensitive") ③
                .value_name("INSENSITIVE")
                .help("Case-insensitive")
                .short("i")
                .long("insensitive")
                .takes_value(false),
        )
        .arg(
            Arg::with_name("recursive") ④
                .value_name("RECURSIVE")
                .help("Recursive search")
                .short("r")
                .long("recursive")
                .takes_value(false),
        )
        .arg(
            Arg::with_name("count") ⑤
                .value_name("COUNT")
                .help("Count occurrences")
                .short("c")
                .long("count")
                .takes_value(false),
        )
        .arg(
            Arg::with_name("invert") ⑥
                .value_name("INVERT")
                .help("Invert match")
                .short("v")
                .long("invert-match")
                .takes_value(false),
        )
        .get_matches();
```

- 1. The first positional argument is for the `pattern`.
- 2. The rest of the positional arguments are for the inputs. The default is "-".
- 3. The `insensitive` flag will handle case-insensitive options.
- 4. The `recursive` flag will handle searching for files in directories.
- 5. The `count` flag will cause the program to print counts.
- 6. The `invert` flag will search for lines not matching the pattern.

**NOTE**

Here the order in which you declare the positional parameters is important as the first one defined will be for the first positional argument. You can still define the options before or after the positional parameters.

Next, I used the arguments to create a regular expression that will incorporate the `insensitive` option:

```
let pattern = matches.value_of("pattern").unwrap(); ①
let pattern = RegexBuilder::new(pattern) ②
    .case_insensitive(matches.is_present("insensitive")) ③
    .build() ④
    .map_err(|_| format!("Invalid pattern \"{}\"", pattern))?; ⑤

    Ok(Config { ⑥
        pattern,
        files: matches.values_of_lossy("files").unwrap(),
        recursive: matches.is_present("recursive"),
        count: matches.is_present("count"),
        invert_match: matches.is_present("invert"),
    })
}
```

- 1. The `pattern` is required, so it should be safe to unwrap the value.
- 2. The `RegexBuilder::new` method will create a new regular expression.
- 3. The `Regex::case_insensitive` method will cause the regex to disregard case in comparisons when the `insensitive` is flag is present.
- 4. The `Regex::build` method will compile the regex.
- 5. If `build` returns an error, use `Result::map_err` to create an error message that the given pattern is invalid.
- 6. Return the `Config`.

RegexBuilder::build will reject any pattern that is not a valid regular expression, and this raises an interesting point. There are many syntaxes for writing regular expressions. If you look closely at the manual page for grep, you'll notice these options:

```
-E, --extended-regexp
    Interpret pattern as an extended regular expression
```

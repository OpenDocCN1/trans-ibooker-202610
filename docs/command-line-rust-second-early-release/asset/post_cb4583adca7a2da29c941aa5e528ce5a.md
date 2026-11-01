
The cat utility exits 0 on success, and >0 if an error occurs.

## The GNU version does respond to --help:

```
$ cat --help
Usage: cat [OPTION]... [FILE]...
Concatenate FILE(s), or standard input, to standard output.

  -A, --show-all          equivalent to -vET
  -b, --number-nonblank   number nonempty output lines,
overrides -n
  -e                      equivalent to -vE
  -E, --show-ends         display $ at end of each line
  -n, --number            number all output lines
  -s, --squeeze-blank     suppress repeated empty output
lines
  -t                      equivalent to -vT
  -T, --show-tabs         display TAB characters as ^I
  -u                      (ignored)
  -v, --show-nonprinting  use ^ and M- notation, except
for LFD and TAB
      --help    display this help and exit
      --version output version information and exit
```

With no FILE, or when FILE is -, read standard input.

Examples:

cat f - g  Output f's contents, then standard input, then g's contents.
cat        Copy standard input to standard output.

GNU coreutils online help:
<http://www.gnu.org/software/coreutils/>
For complete documentation, run: info coreutils 'cat invocation'

> NOTE
> 
> The BSD version predates the GNU version, so the latter implements all the same short flags to be compatible. As is typical of GNU programs, it also offers long flag aliases like `--number` for `-n` and `--number-nonblank` for `-b`. I will show you how to offer both options like the GNU version.

For the challenge program, I will only implement the options `-b|--number-nonblank` and `-n|--number`. I will also show how to read regular files and `STDIN` when given a filename argument of "-". I've put four files for testing into the `03_catr/tests/inputs` directory:

- 1. *empty.txt*: an empty file
- 2. *fox.txt*: a single line of text
- 3. *spiders.txt*: three lines of text
- 4. *the-bustle.txt*: a lovely poem by Emily Dickinson that has nine lines including one blank

Empty files are common, if useless. I include this to ensure my program can gracefully handle unexpected input. That is, I want my program to at least not fall over. The following command produces no output, so I expect my program to do the same:

```
$ cd 03_catr
$ cat tests/inputs/empty.txt
```

Next, I'll run `cat` on a file with one line of text:

```
$ cat tests/inputs/fox.txt
The quick brown fox jumps over the lazy dog.
```

The `-n|--number` and `-b|--number-nonblank` flags will both number the lines, and the line number is right-justified in a field six characters wide followed by a tab character and then the line of text.

To distinguish the tab character, I can use the -t option to display non-printing characters so that the tab shows as ^I. In the following command, I use the Unix pipe | to connect STDOUT from the first command to STDIN in the second command:

```
$ cat -n tests/inputs/fox.txt | cat -t
    1^IThe quick brown fox jumps over the lazy dog.
```

The spiders.txt file has three lines of text which should be numbered with the -b option:

```
$ cat -b tests/inputs/spiders.txt
    1  Don't worry, spiders,
    2  I keep house
    3  casually.
```

The difference between -n (on the left) and -b (on the right) is apparent only with the-bustle.txt as the latter will only number nonblank lines:

```
$ cat -n tests/inputs/the-bustle.txt      $ cat -b tests/inputs/the-bustle.txt
    1  The bustle in a house                1  The bustle in a house
    2  The morning after death              2  The morning after death
    3  Is solemnest of industries           3  Is solemnest of industries
    4  Enacted upon earth,−                 4  Enacted upon earth,−
    5
    6  The sweeping up the heart,           5  The sweeping up the heart,
    7  And putting love away                6  And putting love away
    8  We shall not want to use again       7  We shall not want to use again
    9  Until eternity.                      8  Until eternity.
```

## NOTE

Oddly, you can use -b and -n together, and the -b option takes precedence. The challenge program will allow only one or the other.

When processing any file that does not exist or cannot be opened, cat will print a message to STDERR and move to the next file. In the following example, I’m using blargh as a nonexistent file. I create the file cant-touch-this using the touch command and use the chmod command to set the file with the permissions that make it unreadable. You’ll learn more about what the 000 means in Chapter 15 when you write a clone of ls:

```
$ touch cant-touch-this && chmod 000 cant-touch-this
$ cat tests/inputs/fox.txt blargh tests/inputs/spiders.txt cant-touch-this
The quick brown fox jumps over the lazy dog. ①
cat: blargh: No such file or directory ②
Don't worry, spiders, ③
I keep house
casually.
cat: cant-touch-this: Permission denied ④
```

- ① This is the output from the first file.
- ② This is an error for a nonexistent file.
- ③ This is the output from the third file.
- ④ This is the error for an unreadable file.

Finally, run cat with all the files and notice that it starts renumbering the lines for each file:

```
$ cd tests/inputs
$ cat -n empty.txt fox.txt spiders.txt the-bustle.txt
```

```
1  The quick brown fox jumps over the lazy dog.
1  Don't worry, spiders,
2  I keep house
3  casually.
1  The bustle in a house
2  The morning after death
3  Is solemnest of industries
4  Enacted upon earth,–
5
6  The sweeping up the heart,
7  And putting love away
8  We shall not want to use again
9  Until eternity.
```

If you look at the *mk-outs.sh* script, you’ll see I execute `cat` with all these files, individually and together, as regular files and through `STDIN`, using no flags and with the `-n` and `-b` flags. I capture all the outputs to various files in the *tests/expected* directory to use in testing.

## Getting Started with Test-Driven Development

In Chapter 2, I wrote the tests at the end of the chapter because I needed to show you some basics of the language. Starting with this exercise, I want to make you think about *test-driven development* (TDD) as described in a book by that title written by Kent Beck (Addison-Wesley, 2002). TDD advocates writing tests for code *before* writing the code as shown in Figure 3-1.

- Add a test
- Run all tests and see that the new test fails
- Write the code
- Run tests
- Refactor code
- Repeat

*Figure 3-1. The test-driven development cycle*

## NOTE

Technically, TDD involves writing each test as you add a feature. Since I've written all the tests for you, you might consider this more like *test-first development*. Once you've written code that passes the tests, you can start to refactor your code to improve it, perhaps by shortening the lines of code or by finding a faster implementation.

The challenge program you write should be called `catr` (pronounced *cat-er*) for a Rust version of `cat`. I suggest you begin with `cargo new catr` to start a new application and then copy my *03_catr/tests* directory into your source tree. Don't copy anything but the tests as you will write the rest of the code yourself. You should have a structure like this:

```
$ tree -L 2 catr/
catr
├── Cargo.toml
├── src
│   └── main.rs
└── tests
    ├── cli.rs
    ├── expected
    └── inputs

4 directories, 3 files
```

I'm going to use all the same external crates as in Chapter 2 plus the `rand` crate for testing, so update your *Cargo.toml* to this:

```
[dependencies]
clap = "2.33"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

Now run `cargo test` to download the crates, compile your program, and run the tests. All the tests should fail. Your mission, should you choose to accept it, is to write a program that will pass these tests.

## Creating a Library Crate

The program in Chapter 2 was quite short and easily fit into *src/main.rs*. The typical programs you will write in your career will likely be much longer. Starting with this program, I will divide my code into a library in *src/lib.rs* and a binary in *src/main.rs* that will call library functions. I believe this organization makes it easier to test and grow applications over time.

First, I’ll move all the important bits from *src/main.rs* into a function called `run` in *src/lib.rs*. This function will return a kind of `Result` to indicate success or failure. This is similar to the `TestResult` type alias from Chapter 2. The `TestResult` always returns the unit type `()` in the `Ok` variant, but `MyResult` can return an `Ok` that contains any type which I can denote using the generic `T`:

```
use std::error::Error; ①

type MyResult<T> = Result<T, Box<dyn Error>>;②

pub fn run() -> MyResult<()> { ③
    println!("Hello, world!"); ④
    Ok(()) ⑤
}
```

- ① Import the `Error` trait for representing error values.
- ② Create a `MyResult` to represent an `Ok` value for any type `T` or some `Err` value that implements the `Error` trait.

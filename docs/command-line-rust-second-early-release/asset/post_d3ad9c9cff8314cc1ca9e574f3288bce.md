
Read either `STDIN` if the input file is a dash or open the given filename. Create an informative error message when this fails.

- 2. Create a new, empty mutable `String` buffer to hold each line.
- 3. Create an infinite loop.
- 4. Read a line of text while preserving the line endings.
- 5. If no bytes were read, break out of the loop.
- 6. Print the line buffer.
- 7. Clear the line buffer.

Run your program with an input file to ensure it works:

```
$ cargo run -- tests/inputs/one.txt
a
```

It should also work for reading `STDIN`:

```
$ cargo run -- - < tests/inputs/one.txt
a
```

Try to get your program to iterate the lines of input and count each unique run of lines, then try to get your program to print the lines with and without the counts. Look at how `open` works, and try to copy those ideas to use `File::create` to write output to a file or `STDOUT`. Remember that you can run just a subset of tests with a command like `cargo test empty` to run all the tests with the string *empty* in the name. I know you can do this.

## Solution

I'll step you through how I arrived at a solution. Your version may be different, but it's fine as long as it passes the test suite. Building on the last version I showed, I decided to create two additional mutable variables to hold the last line of text and the running count. For now, I will always print the count to make sure it's working correctly:

```
pub fn run(config: Config) -> MyResult<()> {
    let mut file = open(&config.in_file)
        .map_err(|e| format!("{}: {}", config.in_file,
 e))?;
    let mut line = String::new();
    let mut last = String::new(); ①
    let mut count: u64 = 0; ②

    loop {
        let bytes = file.read_line(&mut line)?;
        if bytes == 0 {
            break;
        }

        if line.trim_end() != last.trim_end() { ③
            if count > 0 { ④
                print!(" {:>4} {}", count, last); ⑤
            }
            last = line.clone(); ⑥
            count = 0; ⑦
        }

        count += 1; ⑧
        line.clear();
    }

    if count > 0 { ⑨
        print!(" {:>4} {}", count, last);
    }

    Ok(())
}
```

- ① Create a mutable variable to hold the last line of text.
- ② Create a mutable variable to hold the count.

- 3. Compare the current line to the last time, both trimmed of any possible trailing whitespace.
- 4. Only print the output when `count` is greater than 0.
- 5. Print the `count` right-justified in a column 4 characters wide followed by a space and the `line` value.
- 6. Set the `last` variable to a copy of the current `line`.
- 7. Reset the counter to 0.
- 8. Increment the counter by 1.
- 9. Handle the last line of the file.

If I run `cargo test`, this will pass a goodly number of tests. This code is clunky, though. I don't like having to check `if count > 0` twice as it violates the *don't repeat yourself* (DRY) principle, so I'm thinking that needs to be put into a function. I'm also always printing the count, but I should only print this when `config.count` is `true` —another condition that could go into this function. I decided to write this as a closure inside the `run` function to *close around* the `config.count` value:

```
```rust
let print = |count: &u64, line: &String| {
    if count > &0 {
        if config.count {
            print!("{:>4} {}", &count, &line);
        } else {
            print!("{}", &line);
        }
    };
};
```
```

- 1. The `print` closure will accept `count` and `line` values.

- 2. Only print if the `count` is greater than 0.
- 3. Check if the `config.count` value is `true`.
- 4. Use the `print!` macro to print the `count` and `line` to `STDOUT`.
- 5. Otherwise, print the `line` to `STDOUT`.

## CLOSURES VERSUS FUNCTIONS

A closure is a function, so you might be tempted to write print as a function inside the run function:

```
pub fn run(config: Config) -> MyResult<()> {
    ...
    fn print(count: &u64, line: &String) {
        if count > &0 {
            if config.count {
                print!("{:>4} {}", &count, &line);
            } else {
                print!("{}", &line);
            }
        }
    };
};
...
```

This is a common way to write a closure in other languages, and Rust does allow you to declare a function inside another function; however, the Rust compiler specifically disallows capturing a dynamic value from the environment¹:

```
error[E0434]: can't capture dynamic environment in a fn item
  --> src/lib.rs:67:16
   |
67 |             if config.count {
   |                    ^^^^^^
   |
   = help: use the `|| { ... }` closure form instead
```

I can update the rest of the function to use this closure:

```
loop {
    let bytes = file.read_line(&mut line)?;
    if bytes == 0 {
        break;
    }
}
```

```
```rust
if line.trim_end() != last.trim_end() {
    print(&count, &last);
    last = line.clone();
    count = 0;
}

count += 1;
line.clear();
}

print(&count, &last);
```
```

With these changes, my program will pass several more tests. If I look at the failed tests, they all contain outfile because I’m failing to write a named output file. I can add this last feature with two changes. First, I would like to open the output file in the same way as I open the input file by either creating a named output file or by using STDOUT. You’ll need to add use std::io::Write for the following code, which you can place just after the file variable:

```
```rust
let mut out_file: Box<dyn Write> = match &config.out_file {
    Some(out_name) => Box::new(File::create(&out_name)?),
    _ => Box::new(io::stdout()),
};
```
```

① The mutable out_file will be a boxed value that implements the std::io::Write trait.

② When config.out_file is Some filename, use File::create to try to create the file.

③ Otherwise, use std::io::stdout.

If you follow the documentation for both File::create and io::stdout, you’ll see both have a Traits section showing the

various traits they implement. Both show that they implement Write, and so they satisfy the type requirement Box<dyn Write> which says that the value inside the Box must implement this trait.

The second change I need to make is to use out_file for the output. I will replace the print! macro with write! to write the output to a stream like a filehandle or STDOUT. The first argument to write! must be a mutable value that implements the Write trait.

The documentation shows that write! will return a std::io::Result because it might fail. As such, I changed my print closure to return MyResult. Here is the final version of my run function that passes all the tests:

```
pub fn run(config: Config) -> MyResult<()> {
    let mut file = open(&config.in_file)
        .map_err(|e| format!("{}: {}", config.in_file,
    e))?; ①
    let mut out_file: Box<dyn Write> = match
    &config.out_file { ②
        Some(out_name) =>
    Box::new(File::create(&out_name)?),
        _ => Box::new(io::stdout()),
    };

    let mut print = |count: &u64, line: &String| ->
    MyResult<()> { ③
        if count > &0 {
            if config.count {
                write!(out_file, "{:>4} {}", &count,
    &line)?;
            } else {
                write!(out_file, "{}", &line)?;
            }
        }
        Ok(())
    };

    let mut line = String::new();
    let mut last = String::new();
    let mut count: u64 = 0;
    loop {
```

```
let bytes = file.read_line(&mut line)?;
if bytes == 0 {
    break;
}

if line.trim_end() != last.trim_end() {
    print(&count, &last)?; ④
    last = line.clone();
    count = 0;
}

count += 1;
line.clear();
}

print(&count, &last)?; ⑤

Ok(())
}
```

- 1. Open either STDIN or the given input filename.
- 2. Open either STDOUT or the given output filename.
- 3. Create a mutable print closure to format the output.
- 4. Use the print closure to possibly print output. Use ? to propagate potential errors.
- 5. Handle the last line of the file.

I would think it’s highly unlikely that you would independently write something exactly like my solution. As long as your solution passes the tests, it’s perfectly acceptable. Part of what I like about writing with tests is that there is an objective determination when a program meets some level of specifications. Louis Srygley says, “Without requirements or design, programming is the art of adding bugs to an empty text file.” I would say that tests are the requirements made incarnate. Without tests, you simply have no way to know when a

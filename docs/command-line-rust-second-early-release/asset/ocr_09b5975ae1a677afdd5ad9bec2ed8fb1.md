In the parlance of program parameters, the -n is an optional argument because you can leave it out. Typically program options start with one or two dashes. It's common to have short names with one dash and a single character like -h for the help flag and long names with two dashes and a word like --help. Specifically, -n and -h are flags that have one meaning when present and the opposite when absent. In this case, -n says to omit the trailing newline; otherwise, print as normal.

All the other arguments to echo are positional because their position relative to the name of the program (the first element in the arguments) determines their meaning. Consider the command chmod that takes two positional arguments, a mode like 755 first and a file or directory name second. In the case of echo, all the positional arguments are interpreted as the text to print, and they should be printed in the same order they are given. This is not a bad start, but the arguments to the programs in this book are going to become much more complex. I need to find a better way to parse the program's arguments.

# Adding clap as a Dependency

Although there are various methods and crates for parsing command-line arguments, I will exclusively use the clap crate in this book. To get started, I need to tell Cargo that I want to download this crate and use it in my project. I can do this by adding a dependency to Cargo.toml. Edit your file to add clap version 2.33 to the [dependencies] section:

```
$ cat Cargo.toml
[package]
name = "echor"
version = "0.1.0"
edition = "2018"
```
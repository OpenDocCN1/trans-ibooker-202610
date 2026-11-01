Import the `clap::App` struct.

+   2. Create a new `App` with the name *echor*.
+   3. Use semantic version numbers as described earlier.
+   4. Your name and email address so people know where to send the money.
+   5. This is a short description of the program.
+   6. Ask the `App` to parse the arguments.

> **NOTE**
> 
> In the preceding code, the leading underscore in the variable name `_matches` is functional. It tells the Rust compiler that I do not intend to use this variable right now. Without the underscore, the compiler would warn about an unused variable.

With this code in place, I can run the program with the `-h` or `--help` flags to get a usage document. Note that I didn’t have to define this argument as `clap` did this for me:

```
$ cargo run -- -h
echor 0.1.0 ①
Ken Youens-Clark <kyclark@gmail.com> ②
Rust echo ③

USAGE:
    echor

FLAGS:
    -h, --help      Prints help information
    -V, --version   Prints version information
```

① The app name and `version` number appear here.
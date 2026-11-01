+   2. Create a new `Arg` with the name `text`. This is a required positional argument that must appear at least once and can be repeated.
+   
+   3. Create a new `Arg` with the name `omit_newline`. This is a flag that has only the short name `-n` and takes no value.
+   
+   4. Parse the arguments and return the matching elements.
+   
+   5. Pretty-print the arguments.

> **NOTE**
> 
> Earlier I used `{ :? }` to format the debug view of the arguments. Here I’m using `{ :#? }` to include newlines and indentations to help me read the output. This is called *pretty-printing* because, well, it’s prettier.

If you request the usage again, you will see the new parameters:

```
$ cargo run -- --help
 ech or 0.1.0
 Ken Youens-Clark <k yclark@gmail.com>
 Rust echo

 USAGE:
     echor [FLAGS] <TEXT>...

 FLAGS:
     -h, --help          Prints help information
     -n                  Do not print newline ①
     -V, --version       Prints version information

 ARGS:
     <TEXT>...           Input text ②
```

1. The `-n` flag to omit the newline is optional.
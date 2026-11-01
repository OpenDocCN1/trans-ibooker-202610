Run your program and verify your code gracefully prints error messages for bad input files and continues to process the valid ones:

```
$ cargo run -- blargh cant-touch-this tests/inputs/fox.txt
Failed to open blargh: No such file or directory (os error 2)
Failed to open cant-touch-this: Permission denied (os error 13)
Opened tests/inputs/fox.txt
```

With this addition, you should be able to pass **cargo test skips_bad_file**. Now that you are able to open and read valid input files, I want you to finish the program on your own. Can you figure out how to read the opened file line-by-line? Start with *tests/inputs/fox.txt* that has only one line. You should be able to see the following output:

```
$ cargo run -- tests/inputs/fox.txt
The quick brown fox jumps over the lazy dog.
```

Verify that you can read `STDIN` by default. In the following command, I will use the `|` to pipe `STDOUT` from the first command to the `STDIN` of the second command:

```
$ cat tests/inputs/fox.txt | cargo run
The quick brown fox jumps over the lazy dog.
```

The output should be the same when providing the filename “-”. In the following command, I will use the `bash` redirect operator `<` to take input from the given filename and provide it to `STDIN`:

```
$ cargo run -- - < tests/inputs/fox.txt
The quick brown fox jumps over the lazy dog.
```

Next, try an input file with more than one line and try to number the lines for `-n`:
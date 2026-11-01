```
$ cargo run -- tests/inputs/fox.txt
    1       9       48 tests/inputs/fox.txt
```

Be sure your program can handle the Unicode in *tests/inputs/atlamal.txt* correctly:

```
$ cargo run -- tests/inputs/atlamal.txt
    4       29      177 tests/inputs/atlamal.txt
```

And that you correctly count the characters:

```
$ cargo run -- tests/inputs/atlamal.txt -wml
    4       29      159 tests/inputs/atlamal.txt
```

When you can handle any one file, use more than one to check that you print the correct *total* column:

```
$ cargo run -- tests/inputs/*.txt
    4       29      177 tests/inputs/atlamal.txt
    0       0       0 tests/inputs/empty.txt
    1       9       48 tests/inputs/fox.txt
    5       38      225 total
```

When all that works correctly, try reading from STDIN:

```
$ cat tests/inputs/atlamal.txt | cargo run
    4       29      177
```

Run `cargo test` often to see how you’re progressing. Don’t read ahead until you pass all the tests.

# Solution

I’d like to walk you through how I arrived at a solution, and I’d like to stress that mine is just one of many possible ways to write this program. As long as your code passes the tests and produces the
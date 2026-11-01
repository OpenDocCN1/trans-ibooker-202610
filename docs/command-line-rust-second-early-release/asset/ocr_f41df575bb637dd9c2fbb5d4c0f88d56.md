```
$ cargo run -- -icr the tests/inputs
tests/inputs/empty.txt:0
tests/inputs/nobody.txt:3
tests/inputs/bustle.txt:3
tests/inputs/fox.txt:1
```

Handle errors like nonexistent files while processing the files in order:

```
$ cargo run -- fox blargh tests/inputs/fox.txt
blargh: No such file or directory (os error 2)
tests/inputs/fox.txt:The quick brown fox jumps over the
lazy dog.
```

Another potential problem you should gracefully handle is a failure to open a file perhaps due to insufficient permissions:

```
$ touch hammer && chmod 000 hammer
$ cargo run -- fox hammer tests/inputs/fox.txt
hammer: Permission denied (os error 13)
tests/inputs/fox.txt:The quick brown fox jumps over the
lazy dog.
```

These challenges are getting harder, so it's OK to feel a bit overwhelmed by the requirements. Try to tackle each task in order, and keep running cargo test to see how many you're able to pass. When you get stuck, run grep with the arguments and closely examine the output. Then run your program with the same arguments and try to find the differences.

# Solution

To start, I'll share my find_files function:

```
fn find_files(files: &[String], recursive: bool) ->
Vec<MyResult<String>> {
    let mut results = vec![]; ①
```
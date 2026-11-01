The next function tests both reading from STDIN and counting the lines:

```
```rust
fn run_stdin_count(test: &Test) -> TestResult {
    let input = fs::read_to_string(test.input)?;
    let expected = fs::read_to_string(test.out_count)?;
    Command::cargo_bin(PRG)? ①
        .arg("--count")
        .write_stdin(input)
        .assert()
        .success()
        .stdout(expected);
    Ok(())
}
```
```

① Run the program with the long --count flag and feed the input to STDIN, verify that STDOUT is correct.

The next function checks that the program accepts both the input and output files as positional arguments. This is somewhat more interesting as I needed to use temporary files in the testing because, as you have seen repeatedly, Rust will run the tests in parallel. If I were to use the same dummy filename like blargh to write all the output files, the tests would overwrite each other’s output. To get around this, I use the tempfile::NamedTempFile to get a dynamically generated temporary filename that will automatically be removed when I finish:

```
```rust
fn run_outfile(test: &Test) -> TestResult {
    let expected = fs::read_to_string(test.out)?;
    let outfile = NamedTempFile::new()?; ①
    let outpath = &outfile.path().to_str().unwrap(); ②

    Command::cargo_bin(PRG)? ③
        .args(&[test.input, outpath])
        .assert()
        .success()
        .stdout("");
    let contents = fs::read_to_string(&outpath)?; ④
```
```
...
36 |     if fs::metadata(filename).is_err() {
|         ------- value moved here
37 |     return filename;
|         ^^^^^^^^ value used here after move
```

Don’t worry if you don’t completely understand the preceding code yet. I’m only showing this so you understand how it is used in the `skips_bad_file` test:

```
#[test]
fn skips_bad_file() -> TestResult {
    let bad = gen_bad_file(); ①
    let expected = format!("{}: .* [()]os error 2[]]", bad);
②
    Command::cargo_bin(PRG)? ③
        .arg(&bad)
        .assert()
        .success() ④
        .stderr(predicate::str::is_match(expected)?);
    Ok(())
}
```

+   ① Generate the name of a nonexistent file.
+   
+   ② The expected error message should include the filename and the string “os error 2” on both Windows or Unix platforms.
+   
+   ③ Run the program with the bad file and verify that STDERR matches the expected pattern.
+   
+   ④ The command should succeed as bad files should only generate warnings and not kill the process.
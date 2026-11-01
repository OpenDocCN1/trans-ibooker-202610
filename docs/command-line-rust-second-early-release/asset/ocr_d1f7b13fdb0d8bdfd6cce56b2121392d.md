```
fn find_files(files: &[String], recursive: bool) ->
Vec<MyResult<String>> {
    unimplemented!();
}
```

To test this, I can add a `tests` module to `src/lib.rs`. Note that this will use the `rand` module which should be listed in the `[dev-dependencies]` section of your *Cargo.toml* as noted earlier in the chapter:

```
#[cfg(test)]
mod tests {
    use super::find_files;
    use rand::{distributions::Alphanumeric, Rng};

    #[test]
    fn test_find_files() {
        // Verify that the function finds a file known to exist
        let files =
            find_files(&
["./tests/inputs/fox.txt".to_string()], false);
        assert_eq!(files.len(), 1);
        assert_eq!(files[0].as_ref().unwrap(),
"./tests/inputs/fox.txt");

        // The function should reject a directory without
the recursive option
        let files = find_files(&
["./tests/inputs".to_string()], false);
        assert_eq!(files.len(), 1);
        if let Err(e) = &files[0] {
            assert_eq!(
                e.to_string(),
                "./tests/inputs is a directory".to_string()
            );
        }

        // Verify the function recurses to find four files
in the directory
        let res = find_files(&
["./tests/inputs".to_string()], true);
        let mut files: Vec<String> = res
```
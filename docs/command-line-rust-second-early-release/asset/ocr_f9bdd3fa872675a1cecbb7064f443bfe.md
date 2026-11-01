8 Collect the strings into a vector and join them into a new string for printing.

If I run this program, I will see the following output:

```
$ cargo run
Author      Year    Title
Émile Zola  1865    La Confession de
Claude
Samuel Beckett  1952    Waiting for Godot
Jules Verne 1870    20,000 Leagues
Under the Sea
```

Coming back to the challenge program, think about how you might use some of these ideas to write a function like `extract_fields` that accepts a `StringRecord` and pulls out the fields found in the PositionList:

```
fn extract_fields(record: &StringRecord, field_pos: & [usize]) -> Vec<String> {
    unimplemented!();
}
```

Here is a test you could use:

```
#[test]
fn test_extract_fields() {
    let rec = StringRecord::from(vec!["Captain", "Sham", "12345"]);
    assert_eq!(extract_fields(&rec, &[0]), &["Captain"]);
    assert_eq!(extract_fields(&rec, &[1]), &["Sham"]);
    assert_eq!(extract_fields(&rec, &[0, 2]), &["Captain", "12345"]);
    assert_eq!(extract_fields(&rec, &[0, 3]), &["Captain"]);
    assert_eq!(extract_fields(&rec, &[1, 0]), &["Sham", "Captain"]);
}
```
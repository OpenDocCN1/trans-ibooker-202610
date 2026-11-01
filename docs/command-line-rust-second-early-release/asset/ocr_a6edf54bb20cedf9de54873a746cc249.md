Here is a test you can add to your tests module to help you see how the function might work:

```
#[test]
fn test_extract_chars() {
    assert_eq!(extract_chars("", &[0]), "".to_string());
    assert_eq!(extract_chars("áb c", &[0]),
"á".to_string());
    assert_eq!(extract_chars("áb c", &[0, 2]),
"ác".to_string());
    assert_eq!(extract_chars("áb c", &[0, 1, 2]),
"áb c".to_string());
    assert_eq!(extract_chars("áb c", &[2, 1]),
"cb".to_string());
    assert_eq!(extract_chars("áb c", &[0, 1, 4]),
"áb".to_string());
}
```

I also wrote a similar extract_bytes function to parse out bytes:

```
fn extract_bytes(line: &str, byte_pos: &[usize]) -> String
{
    unimplemented!();
}
```

Here is the test:

```
fn test_extract_bytes() {
    assert_eq!(extract_bytes("áb c", &[0]),
"�".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1]),
"á".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1, 2]),
"áb".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1, 2, 3]),
"áb c".to_string());
    assert_eq!(extract_bytes("áb c", &[3, 2]),
"cb".to_string());
    assert_eq!(extract_bytes("áb c", &[0, 1, 5]),
"á".to_string());
}
```
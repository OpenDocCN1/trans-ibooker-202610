```
#[cfg(test)] ①
mod tests { ②
    use super::{count, FileInfo}; ③
    use std::io::Cursor; ④

    #[test]
    fn test_count() {
        let text = "I don't want the world. I just want your half.\r\n";
        let info = count(Cursor::new(text)); ⑤
        assert!(info.is_ok()); ⑥
        let expected = FileInfo {
            num_lines: 1,
            num_words: 10,
            num_chars: 48,
            num_bytes: 48,
        };
        assert_eq!(info.unwrap(), expected); ⑦
    }
}
```

+   1. The `cfg` enables conditional compilation, so this module will only be compiled when testing.
+   2. Define a new module (`mod`) called `tests` to contain test code.
+   3. Import the `count` function and `FileInfo` struct from the parent module `super`, meaning *next above or higher* and refers to the module above `tests` that contains it.
+   4. Import `std::io::Cursor`.
+   5. Run `count` with the `Cursor`, which implements `BufRead`.
+   6. Ensure the result is `Ok`.
+   7. Compare the result to the expected value. This comparison requires `FileInfo` to derive `PartialEq`.
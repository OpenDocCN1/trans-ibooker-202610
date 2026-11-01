```
```rust
fn main() {
    if let Err(e) = wcr::get_args().and_then(wcr::run) {
        eprintln!("{}", e);
        std::process::exit(1);
    }
}
```
```

Following is a skeleton for *src/lib.rs* you can copy. First, here is how I would define the `Config` to represent the command-line parameters:

```
```rust
use clap::{App, Arg};
use std::error::Error;

type MyResult<T> = Result<T, Box<dyn Error>>;

#[derive(Debug)]
pub struct Config {
    files: Vec<String>, ①
    lines: bool, ②
    words: bool, ③
    bytes: bool, ④
    chars: bool, ⑤
}
```
```

① The `files` will be a vector of strings.

② The `lines` is a Boolean for whether to print the line count.

③ The `words` is a Boolean for whether to print the word count.

④ The `bytes` is a Boolean for whether to print the byte count.

⑤ The `chars` is a Boolean for whether to print the character count.

Here are the two functions you’ll need to get started. I’ll let you fill in the `get_args` from this skeleton:
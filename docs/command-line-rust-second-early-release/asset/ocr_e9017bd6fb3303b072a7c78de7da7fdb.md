```
fn main() {
    if let Err(e) = commr::get_args().and_then(commr::run)
    {
        eprintln!("{}}", e);
        std::process::exit(1);
    }
}
```

You can start src/lib.rs with the following definition for Config:

```
use clap::{App, Arg};
use std::error::Error;

type MyResult<T> = Result<T, Box<dyn Error>>;

#[derive(Debug)]
pub struct Config {
    file1: String, ①
    file2: String, ②
    suppress_col1: bool, ③
    suppress_col2: bool, ④
    suppress_col3: bool, ⑤
    insensitive: bool, ⑥
    delimiter: String, ⑦
}
```

+   1. The first input filename is a String.
+   2. The second input filename is a String.
+   3. A Boolean for whether to suppress the first column of output.
+   4. A Boolean for whether to suppress the second column of output.
+   5. A Boolean for whether to suppress the third column of output.
+   6. A Boolean for whether to perform case-insensitive comparisons.
+   7. The output column delimiter, which will default to a tab.
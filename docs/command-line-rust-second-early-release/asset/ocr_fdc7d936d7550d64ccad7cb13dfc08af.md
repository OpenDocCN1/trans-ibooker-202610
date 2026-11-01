```
fn open(filename: &str) -> MyResult<Box<dyn BufRead>> {
    match filename {
        "-" => Ok(Box::new(BufReader::new(io::stdin()))),
        _ =>
Ok(Box::new(BufReader::new(File::open(filename)?))),
    }
}
```

Be sure to add all the require dependencies:

```
use clap::{App, Arg};
use std::error::Error;
use std::fs::File;
use std::io::{self, BufRead, BufReader, Read};
```

Expand your run function to try opening the files, printing errors as you encounter them:

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files { ①
        match open(&filename) { ②
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(_file) => println!("Opened {}", filename),
        }
    }
    Ok(())
}
```

+   ① Iterate through each of the filenames.
+   ② Attempt to open the filename.
+   ③ Print errors to STDERR.
+   ④ Print a message that the file was successfully opened.
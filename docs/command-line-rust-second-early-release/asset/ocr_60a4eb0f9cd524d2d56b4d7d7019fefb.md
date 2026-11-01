+   ③ Either unpack an Ok value from line_result or propagate an error.
+   ④ Print the line.

# NOTE

When reading the lines from a file, you don’t get the lines directly from the filehandle but instead get a std::io::Result, which is a “type is broadly used across std::io for any operation which may produce an error.” Reading and writing files falls into the category of IO (input/output) which depends on external resources like the operating and file systems. While it’s unlikely that reading a line from a filehandle will fail, the point is that it could fail.

If you run cargo test, you should pass about half of the tests, which is not bad for so few lines of code.

# Printing Line Numbers

Next, I’d like to add the printing of line numbers for the -n|--number option. One solution that will likely be familiar to C programmers would be something like this:

```
pub fn run(config: Config) -> MyResult<()> {
    for filename in config.files {
        match open(&filename) {
            Err(err) => eprintln!("{}: {}", filename, err),
            Ok(file) => {
                let mut line_num = 0; ①
                for line_result in file.lines() {
                    let line = line_result?;
                    line_num += 1; ②

                    if config.number_lines { ③
                        println!("{:>6}\t{}", line_num,
line); ④
                    } else {
```
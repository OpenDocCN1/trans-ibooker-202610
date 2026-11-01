Starting with the empty file as one of the arguments will force you to deal with having an uneven number of lines where you will have to advance one of the filehandles while the other stays the same. Then when you use two nonempty files, you’ll have to consider how to read the files until you have matching lines and moving them independently, otherwise.

I'll see you on the flip side after you've written your solution.

# Solution

Adding to the last version I showed, I decided to create iterators for the lines of each of the files that would incorporate a closure to handle case-insensitive comparisons:

```
pub fn run(config: Config) -> MyResult<()> {
    let filename1 = &config.file1;
    let filename2 = &config.file2;

    if filename1.as_str() == "-" && filename2.as_str() ==
"-" {
        return Err(From::from("Both input files cannot be STDIN (\"-\")"));
    }

    let case = |line: String| { ①
        if config.insensitive {
            line.to_lowercase()
        } else {
            line
        }
    };

    let mut lines1 =

open(filename1)?.lines().filter_map(Result::ok).map(case);
②
    let mut lines2 =

open(filename2)?.lines().filter_map(Result::ok).map(case);
```
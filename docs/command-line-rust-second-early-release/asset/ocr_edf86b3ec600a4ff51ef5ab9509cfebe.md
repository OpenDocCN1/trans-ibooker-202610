Once you have written these two functions so that they pass the given test, iterate the lines of input text and use the preceding functions to extract and print the desired characters or bytes from each line. You should be able to pass all but the following when you run `cargo test`:

```
failures:
    csv_f1
    csv_f1_2
    csv_f1_3
    csv_f2
    csv_f2_3
    csv_f3
    tsv_f1
    tsv_f1_2
    tsv_f1_3
    tsv_f2
    tsv_f2_3
    tsv_f3
```

# Parsing Delimited Text Files

To pass the final tests, you will need to learn how to parse delimited text files. As stated earlier, this file format uses some delimiter like a comma or tab to indicate the boundaries of a field. Sometimes the delimiting character may also be part of the data, in which case the field should be enclosed in quotes to escape the delimiter. The easiest way to properly parse delimited text is to use something like the `csv` module. I highly recommend that you first read the tutorial.

Next, consider the following example that shows how you can use this module to parse delimited data. If you would like to compile and run this code, start a new project and use the following for *src/main.rs*. Be sure to add the `csv` dependency to your *Cargo.toml* and copy the *books.csv* file into the project.

```
use csv::StringRecord;
use std::fs::File;
```
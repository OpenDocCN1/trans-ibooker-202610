+   4 Write the extracted fields to the output.
+   5 Iterate the lines of text and print the extract bytes.
+   6 Iterate the lines of text and print the extract characters.

# NOTE

I use `csv::WriterBuilder` to correctly escape enclosed delimiters in fields. None of the tests require this, so you may have found a simpler way to write the output that passes the tests. You will shortly see why I did this.

In the preceding code, you may be curious why I ignore any possible headers in the delimited files. By default, the `csv::Reader` will attempt to parse the first row for the column names, but I don’t need to do anything special with these values in this program. If I used this default behavior, I would have to handle the headers separately from the rest of the records. In this context, it’s easier to treat the first row like any other record.

This program passes all the tests and seems to work pretty well for all the testing input files. Because I’m using the `csv` module to parse delimited text files and write the output, this program will correctly handle delimited text files, unlike the original `cut` programs. I’ll use *tests/inputs/books.csv* again to demonstrate that `cutr` will correctly select a field containing the delimiter and will create output that properly escapes the delimiter:

```
$ cargo run -- -d , -f 1,3 tests/inputs/books.csv
Author,Title
Émile Zola,La Confession de Claude
Samuel Beckett,Waiting for Godot
Jules Verne,"20,000 Leagues Under the Sea"
```
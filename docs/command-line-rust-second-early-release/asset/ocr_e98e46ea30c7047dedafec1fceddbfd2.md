lines unique to FILE1, column two contains lines unique to FILE2,
and column three contains lines common to both files.

+   - -1 suppress column 1 (lines unique to FILE1)
+   - -2 suppress column 2 (lines unique to FILE2)
+   - -3 suppress column 3 (lines that appear in both files)
+   - --check-order check that the input is correctly sorted, even if all input lines are pairable
+   - --nocheck-order do not check that the input is correctly sorted
+   - --output-delimiter=STR separate columns with STR
+   - --total output a summary
+   - -z, --zero-terminated line delimiter is NUL, not newline
+   - --help display this help and exit
+   - --version output version information and exit

Note, comparisons honor the rules specified by 'LC_COLLATE'.

Examples:
  comm -12 file1 file2  Print only lines present in both file1 and file2.
  comm -3 file1 file2  Print lines in file1 not in file2, and vice versa.

At this point, you may be wondering exactly why you’d use this. Consider that you have a list of cities where your favorite band played on their last tour:

```
$ cd 10_commr/tests/inputs/
$ cat cities1.txt
Jackson
Denton
Cincinnati
Boston
Santa Fe
Tucson
```
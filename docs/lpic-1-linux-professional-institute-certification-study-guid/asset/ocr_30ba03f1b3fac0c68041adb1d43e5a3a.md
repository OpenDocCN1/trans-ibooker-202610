You can use these SHA utilities, just like the md5sum program was used in Listing 1.46, to ensure a file’s integrity when it is transferred. That way, file corruption is avoided as well as any malicious modifications to the file.

# Using Regular Expressions

Many commands use regular expressions. A regular expression is a pattern template you define for a utility such as grep, which then uses the pattern to filter text. Employing regular expressions along with text-filtering commands expands your mastery of the Linux command line.

# Using grep

A wonderful tool for sifting text is the grep command. The grep command is powerful in its use of regular expressions, which will help with filtering text files. But before we cover those, peruse Table 1.9 for commonly used grep utility options.

# TABLE 1.9 The grep command's commonly used options

| Short | Long | Description |
| :--- | :--- | :--- |
| -c | --count | Display a count of text file records that contain a PATTERN match. |
| -d action | --directories=action | When a file is a directory, if action is set to read, read the directory as if it were a regular text file; if action is set to skip, ignore the directory; and if action is set to recurse, act as if the -R, -r, or --recursive option was used. |
| -E | --extended-regexp | Designate the PATTERN as an extended regular expression. |
| -i | --ignore-case | Ignore the case in the PATTERN as well as in any text file records. |
| -R, -r | --recursive | Search a directory's contents, and for any sub-directory within the original directory tree, consecutively search its contents as well (recursively). |
| -v | --invert-match | Display only text files records that do not contain a PATTERN match. |
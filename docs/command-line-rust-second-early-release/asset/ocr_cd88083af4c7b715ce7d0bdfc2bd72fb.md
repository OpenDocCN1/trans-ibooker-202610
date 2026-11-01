blocks is n. If
    n is followed by a c, then the primary is true if the
file's size
    is n bytes (characters). Similarly if n is followed
by a scale
    indicator then the file's size is compared to n scaled
as:

k    kilobytes (1024 bytes)
M    megabytes (1024 kilobytes)
G    gigabytes (1024 megabytes)
T    terabytes (1024 gigabytes)
P    petabytes (1024 terabytes)

The find program can also take action on the results. For instance, there is a -delete option to remove an entry. This is useful for finding and removing empty files:

```
$ find . -size 0 -delete
```

I’ve often thought it would be nice to have a -count option to tell me how many items are found the way that uniq -c did in the last chapter. I can, of course, pipe this into wc -l (or, even better, wc r), but consider adding such an option to your program. Finally, I’d recommend you look at the source code for fd, another Rust replacement for find.

# Summary

I hope you have an appreciation now for how complex real-world programs can become. The find program can combine multiple comparisons to help you find, say, the large files eating up your disk or files that haven’t been modified in a long time which can be removed. Consider the skills you learned in this chapter:

+   - You can now use `Arg::possible_values` to constrain argument values to a limited set of strings, saving you time in
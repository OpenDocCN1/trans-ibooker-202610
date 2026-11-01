```
matches.is_present("number_nonblank"),
    })
}
```

+   1. This positional argument is for the files and is required to have at least one value that defaults to "-".
+   2. This is an option that has a short name `-n` and a long name `--number`. It does not take a value because it is a flag. When present, it will tell the program to print line numbers. It cannot occur in conjunction with `-b`.
+   3. The `-b|--number-nonblank` flag controls whether to print line numbers for nonblank lines.
+   4. Because at least one value is required, it should be safe to call `Option::unwrap`.
+   5. The two Boolean options are either present or not.

TIP

Optional arguments have short and/or long names, but positional ones do not. You can define optional arguments before or after positional arguments. Defining positional arguments with `min_values` also implies multiple values but does not for optional parameters.

With this much code, you should be able to pass at least a couple of the tests when you execute `cargo test`. There will be a great deal of output showing you all the failing test output, but don’t despair. You will soon see a fully passing test suite.

# Processing the Files
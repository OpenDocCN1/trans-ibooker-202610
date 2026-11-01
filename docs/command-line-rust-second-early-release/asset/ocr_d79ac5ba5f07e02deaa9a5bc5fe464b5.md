```
Arg::with_name("bytes") ②
    .short("c")
    .long("bytes")
    .value_name("BYTES")
    .takes_value(true)
    .conflicts_with("lines")
    .help("Number of bytes"),
)
.arg(
    Arg::with_name("files") ③
    .value_name("FILE")
    .help("Input file(s)")
    .required(true)
    .default_value("-")
    .min_values(1),
)
.get_matches();
```

① The `lines` option takes a value and defaults to “10.”

② The `bytes` option takes a value, and it conflicts with the `lines` parameter so that they are mutually exclusive.

③ The `files` parameter is positional, required, takes one or more values, and defaults to “-”.

# NOTE

> The `Arg::value_name` will be printed in the usage documentation, so be sure to choose a descriptive name. Don’t confuse this with the `Arg::with_name` that uniquely defines the name of the argument for accessing within your code.

Following is how I can use `parse_positive_int` inside `get_args` to validate `lines` and `bytes`. When the function returns an `Err` variant, I use `?` to propagate the error to `main` and end the program; otherwise, I return the `Config`:
Following is my get_args function so that we can regroup on the task at hand:

```
pub fn get_args() -> MyResult<Config> {
    let matches = App::new("findr")
        .version("0.1.0")
        .author("Ken Youens-Clark <kyclark@gmail.com>")
        .about("Rust find")
        .arg(
            Arg::with_name("dirs") ①
                .value_name("DIR")
                .help("Search directory")
                .default_value(".")
                .min_values(1),
        )
        .arg(
            Arg::with_name("names") ②
                .value_name("NAME")
                .help("Name")
                .short("n")
                .long("name")
                .takes_value(true)
                .multiple(true),
        )
        .arg(
            Arg::with_name("types") ③
                .value_name("TYPE")
                .help("Entry type")
                .short("t")
                .long("type")
                .possible_values(&["f", "d", "l"])
                .takes_value(true)
                .multiple(true),
        )
        .get_matches();
}
```

① The dirs argument requires at least one value and defaults to a dot (.).

② The names option accepts zero or more values.

③ The types option accepts zero or more values of f, d, or l.
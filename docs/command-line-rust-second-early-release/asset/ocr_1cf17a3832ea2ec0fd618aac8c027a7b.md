command, I will use *blargh* as a nonexistent file and will create an unreadable file called *cant-touch-this*:

```
$ touch cant-touch-this && chmod 000 cant-touch-this
$ head blargh cant-touch-this tests/inputs/one.txt
head: blargh: No such file or directory
head: cant-touch-this: Permission denied
==> tests/inputs/one.txt <==
Öne line, four words.
```

This will be as much as the challenge program is expected to recreate.

# Getting Started

You might have anticipated that the program I want you to write will be called `headr` (pronounced *head-er*). Start by running `cargo new headr` and copy my `04_headr/tests` directory into your project directory. Add the following dependencies to your *Cargo.toml*:

```
[dependencies]
clap = "2.33"

[dev-dependencies]
assert_cmd = "1"
predicates = "1"
rand = "0.8"
```

I propose you again split your source code so that *src/main.rs* looks like this:

```
fn main() {
    if let Err(e) = headr::get_args().and_then(headr::run)
    {
        eprintln!("{}", e);
        std::process::exit(1);
    }
}
```
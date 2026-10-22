1. Keep in mind that by the time you read this, the version numbers may be different, but it should still work.

# Example 12-1. Cargo.toml file

```
[package]
name = "hello-wasi"
version = "0.1.0"
edition = "2018"

[dependencies]
wasmtime = "0.28.0"
```

We are going to use the Rust version of the libraries below but there are similar structures for other languages such as C and Python. We will look at the .NET and AssemblyScript versions in subsequent chapters. It is even possible to use the command line version of wasmtime with bash as you can see in Example 12-2.

# Example 12-2. Calling our function from Bash

```
#!/bin/sh
# Based on https://docs.wasmtime.dev/lang-bash.html
function how_old() {
    local x=$(($1))
    local y=$(($2))
    local result=$(wasmtime hello.wat --invoke how_old $x $y 2>/dev/null)
    echo "$result"
}

for num in "2021 2000" "2021 1980" "2021 1960"; do
    set -- $num
    echo "how_old($1, $2) = $(how_old "$1" "$2")"
done

brian@tweezer-2 ~/g/w/s/c/hello-wasi> chmod ogu+rx hello.sh
brian@tweezer-2 ~/g/w/s/c/hello-wasi> ./hello.sh
how_old(2021, 2000) = 21
how_old(2021, 1980) = 41
how_old(2021, 1960) = 61
```

We have greater control over the lifecycle when we use the libraries, classes and structures in the supported development languages. First, as a reminder,
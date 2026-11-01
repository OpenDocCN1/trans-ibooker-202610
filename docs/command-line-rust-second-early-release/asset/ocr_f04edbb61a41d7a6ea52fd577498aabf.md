# How echo Works

The echo program is blissfully simple. I'll review how it works so you can consider all the features your version will need. To start, echo will print its arguments to STDOUT:

```
$ echo Hello
Hello
```

I'm using the bash shell which assumes that any number of spaces delimit the arguments, so arguments that have spaces must be enclosed in quotes. In the following command, I'm providing four words as a single argument:

```
$ echo "Rust has assumed control"
Rust has assumed control
```

Without the quotes, I'm providing four separate arguments. Note that I use a varying number of spaces when I provide the arguments, but echo prints them using a single space between each argument:

```
$ echo Rust  has assumed   control
Rust has assumed control
```

If I want the spaces to be preserved, I must enclose them in quotes:

```
$ echo "Rust  has assumed   control"
Rust  has assumed   control
```

It's extremely common for command-line programs to respond to the flags -h or --help to print a message about how to use the program, the so-called usage because that is usually the first word of the output. If I try that with echo, I just get back the text of the flag:

```
$ echo --help
--help
```
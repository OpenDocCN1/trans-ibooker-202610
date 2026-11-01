That was cool.

# Organizing a Rust Project Directory

In your Rust projects, you will likely write many files of source code and will also use code from sources like crates.io. It would be better to create a directory to contain all this. I'll create a directory called hello, and inside that I'll create a src directory for the Rust source code files:

```
$ rm hello ①
$ mkdir -p hello/src ②
```

① Remove the hello binary so I can make a hello directory.

② The mkdir command will make a directory. The -p option says to create parent directories before creating child directories.

Now I'll move the hello.rs source file into hello/src:

```
$ mv hello.rs hello/src
```

Go into that directory and compile your program again:

```
$ cd hello
$ rustc src/hello.rs
```

You should again have a hello executable in the directory. I will use the tree command (which you might need to install) to show you the contents of my directory:

```
$ tree
.
├── hello
│   └── src
│       └── hello.rs
```
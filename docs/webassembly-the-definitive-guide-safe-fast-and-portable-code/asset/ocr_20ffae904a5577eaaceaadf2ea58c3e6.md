At this point, you can download the binary version of the file. By default it will be called *test.wasm* but you can rename it to whatever you like. We will call it *hello.wasm*.

Another option you have to generate this binary form is to use the WebAssembly Binary Toolkit (WABT)⁴ Consult Appendix A for instructions on installing WABT and other tools we will be using throughout the book.

Included with this installation is a command called wat2wasm. It does what the name says and converts the text file to the binary format.

```
brian@tweezer ~/g/w/s/ch02> wat2wasm hello.wat
brian@tweezer ~/g/w/s/ch02> ls -alF
total 24
drwxr-xr-x  5 brian  staff   160 Sep 13 12:54 ./
drwxr-xr-x  3 brian  staff    96 Sep 13 12:05 ../
-rw-r--r--  1 brian  staff    76 Sep 13 12:07 hello.c
-rw-r--r--  1 brian  staff    45 Sep 13 12:54 hello.wasm
-rw-r--r--  1 brian  staff   200 Sep 13 12:52 hello.wat
```

Look closely. Your eyes are not deceiving you. It does not do a whole lot, but the binary format is only 45 bytes long! I used to do a lot more Java programming and have had class names that were longer than that. We need a way of executing our function now that we are not in a browser. This is easy enough to do with the JavaScript API in Node.js, but we will use a different approach to show off a range of choices.

# Running Wasm in a Repl

Another tool I show you how to install in Appendix A is wasm3, ⁵ a WebAssembly interpreter written in C. It allows you to run Wasm modules and functions either on the command line or via an interactive mode conventionally called a “repl”⁶ by the cool kids.

Once I execute the following command, I am given a wasm3 prompt. I pointed it to my Wasm file so there is only one function I can call, but if
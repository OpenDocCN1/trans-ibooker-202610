
typical “Hello, World!” application. There are no strings! It is easier just to deal with numbers until we introduce some more ideas. So, in the spirit of this style of program, we are showing you enough to see it work without overwhelming you.

The purpose of this inner function is to calculate how old someone is based upon what year they were born and what year it currently is. At this point, you may not be surprised to hear that WebAssembly has no concept of dates nor any ability to request the current time by default. I am expecting that you are wondering what exactly WebAssembly can do! Happily, it can do math. If you give it the current year and the year someone was born, it can absolutely subtract the one from the other and produce a result. Please do not be underwhelmed, we are just isolating things to be clear about what is being provided by which part of the system.

As you may know, a stack is a convenient and widely-used data structure in the software world. It is often described as being like a stack of trays in a cafeteria. The workers will place clean trays on top of any other trays. Customers will take one from the top.

Consider an empty stack as shown in Figure 2-1. We say we push something to the top of the stack and pop it off of the top of the stack. We only ever manipulate this location, so this is not an appropriate data structure if you need to traverse a list. At the same time, there is only one place to look for the things we are interested in, so we do not need to specify locations, indices or keys. It is a fast and efficient structure to manipulate.

![](img/16b7990eb0dbf589db177bdbd2f4a189_37_0.png)

Look back to the list of instructions in our function in Example 2-2. The first one is get_local. The WebAssembly host environment will retrieve the value of the parameter named $year_now and then push it to the stack. Assuming the current year is 2021, the result is shown in Figure 2-2.

![](img/16b7990eb0dbf589db177bdbd2f4a189_38_0.png)

Figure 2-2. A stack with one value

At this point, the WebAssembly host environment will advance to the second instruction. It is also a get_local instruction and will retrieve the value of the parameter named $year_born and push it to the stack. The stack will now have two values on it, but the top of the stack points to the newest value pushed. Assuming the person who invoked the function was born in 2000, the stack will look like Figure 2-3.

![](img/16b7990eb0dbf589db177bdbd2f4a189_38_1.png)

Figure 2-3. A stack with two values

The execution environment will press on as there is another instruction. This one is i32.sub. It represents the arithmetic subtraction of one i32 value from another. As it needs two values to make sense, it will consult the top two values on the stack by popping them off resulting in an empty stack looking again like Figure 2-1. It then subtracts the second parameter from the first and pushes the result back to the top of the stack. The result is seen in Figure 2-4.

![](img/16b7990eb0dbf589db177bdbd2f4a189_38_2.png)

Figure 2-4. The result of the subtraction pushed back to the stack

At this point, there are no more instructions to execute and we are left with a single value at the top of the stack. In Example 2-2 we see that our function defines an i32 return value. Whatever is at the top of the stack will be returned as the result of invoking the function.

This may seem like a lot of work to add two numbers, but consider that we have expressed a mathematical sequence of events in a platform-neutral

way. When the code is ultimately converted to native instructions in a runtime host, the values will be loaded into CPU registers and an instruction will add them together using the mechanics of the CPU’s instruction set. We do not have to worry about the details or idiosyncracies of target platforms, but the conversion process will be fast and easy to conduct when it is time. Before that happens, however, we need to convert our text format to its binary representation.

## Converting Wat to Wasm

Anyone who has been a programmer for more than a short time will notice all manner of potential problems with our implementation. We do not handle the case of someone inverting the parameters so that the function would return a negative number. In the interest of keeping the example simple, we are simply ignoring these realities. While this is not a super exciting function, we have investigated the mechanics of expressing some basic behavior via WebAssembly’s native text format. The next step is to turn it into its binary executable form. You have several options for doing this, but we will focus on two approaches.

The first does not require you to install anything. In fact, you can go ahead and invoke your function to see it work! If you go to https://webassembly.github.io/wabt/demo/wat2wasm/index.html you will see a multi-panel site. The upper left corner represents a .wat file. The upper right corner represents an annotated hex dump of the compiled .wat file. The lower left corner represents JavaScript code to invoke the behavior using the API we will introduce more fully later. The lower right corner represents the output from executing the code.

Copy and paste the code from Example 2-2 into the upper left panel labeled WAT. This will cause the text format to be converted into the binary format. Assuming you do not have any typos, you will also be able to download the binary format by pressing the Download button on that same panel. Do not worry about doing that yet.

Now, copy the code from Example 2-3 into the lower left panel. This will invoke the WebAssembly JavaScript API available in most modern browsers (and Node.js). We will discuss it more later, but for now we are retrieving the bytes of the binary module (available here via the wasmModule variable) and getting a reference to the how_old function so we can call it. As you can see, this function can be invoked like any other JavaScript function. The result of doing so will be printed out via console.log() to the lower right panel.

### Example 2-3. Some JavaScript to invoke our function

```
const wasmInstance = new WebAssembly.Instance(wasmModule, {});
const { how_old } = wasmInstance.exports;
console.log(how_old(2021, 2000));
```

If everything goes well, you should see something like the screen shot in Figure 2-5. Try changing the dates for the current year and birth year parameters and make sure that our math is correct.

![](img/16b7990eb0dbf589db177bdbd2f4a189_40_0.png)

Figure 2-5. Converting a WebAssembly text file into a binary file and executing it

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

## Running Wasm in a Repl

Another tool I show you how to install in Appendix A is wasm3, ⁵ a WebAssembly interpreter written in C. It allows you to run Wasm modules and functions either on the command line or via an interactive mode conventionally called a “repl”⁶ by the cool kids.

Once I execute the following command, I am given a wasm3 prompt. I pointed it to my Wasm file so there is only one function I can call, but if

```
written
    )
    drop ;; Discard the number of bytes written from the top of the stack
)
)
```

The Unix pipes and filters approach to tool composition allows you to redirect the output of one program to the input of another one⁷. Examples include using the less command to pause scrolling terminal messages or the grep command to find elements in the process table that match a certain pattern.

Files are the common abstraction in Unix for both real files on disk as well as virtual files such as the input or output to a program. The ability to print to the console is managed by writing to a particular file descriptor. In this environment, file descriptor 0 in a program represents the standard input, what was passed into the program. File descriptor 1 represents the standard output, what the program logs out under normal circumstances. File descriptor 2 is standard error which is usually used for error messages.

It has been a few chapters since we messed around with Wat and this is slightly more complicated than what we have seen so far, but I think you should be able to sort through it with some guidance.

The first thing we do is to import a function called fd_write from the wasi_unstable namespace. This is where a WASI environment will meet our expectations if it wants to. If we are not provided with this behavior, we will not be able to execute.

The fd_write function takes four parameters that indicate which file descriptor to use, where we have stored a string to write in memory, how long it is and where to write the number of bytes written. As specified, this will be after our strings in memory.

We next define our module’s Memory instance and export it. The function we just imported is going to want to interrogate this based on the parameters we send it. We use a data element to write the string “hello
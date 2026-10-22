```
(module
    (import "linking2" "double" (func $double (param i32) (result i32)))
    (import "linking2" "log" (func $log (param i32 i32)))
    (import "linking2" "memory" (memory 1))
    (import "linking2" "memory_offset" (global $offset i32))

    (func (export "run")
        ;; Call into the other module to double our number, and we could print it
        ;; here but for now we just drop it
        i32.const 2
        call $double
        drop

        ;; Our `data` segment initialized our imported memory, so let's print the
        ;; string there now.
        global.get $offset
        i32.const 14
        call $log
    )

    (data (global.get $offset) "Hello, world!\n")
)
```

The second module is shown in Example 12-13. It defines a type for a function that takes four i32 parameter and returns an i32. This type will correspond to the fd_write method that we will import from the WASI namespace "wasi_snapshot_preview1". This method, as we saw in Chapter 11 takes parameters for the file description, where the string vectors start, how many there are and where it should write the return value representing the number of bytes written.

There is also a simple function called double that loads the i32 parameter sent in to the stack, follows up with the constant “2” and then invokes the i32.mul instruction which will pop the top two stack values, multiply them, and write the result back to the top of the stack.

Our exported log function calls the imported fd_write after setting up the details. Notice many modules might import the fd_write functionality, but here we have a reusable function that hides most of the
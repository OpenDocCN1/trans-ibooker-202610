```
Finished dev [unoptimized + debuginfo] target(s) in 0.03s
Running `/Users/brian/.cargo/bin/cargo-wasi target/wasm32-wasi/debug/hello-world.wasm`
Running `target/wasm32-wasi/debug/hello-world.wasm`
Hello, world!
```

The actual details of how this all works is more complicated than I want to get into at the moment, but to give you a bit of a peek behind the curtain, Example 11-2 is an example from the Wasmtime WASI tutorial⁶. It is working at a lower level than perhaps you have done before, but it is fairly typical systems programming.

# Example 11-2. Invoking Standard Library Behavior from Wat

```
;; Taken from the Wasmtime WASI Tutorial
(module
    ;; Import the required fd_write WASI function which will write
    the given io
    ;; vectors to stdout. The function signature for fd_write is:
    ;; (File Descriptor, *iovs, iovs_len, nwritten) ->&gt; Returns #
    of bytes written
    (import "wasi_unstable" "fd_write"
        (func $fd_write (param i32 i32 i32 i32) (result i32)))

    (memory 1)
    (export "memory" (memory 0))

    ;; Write 'hello world\n' to memory at an offset of 8 bytes
    ;; Note the trailing newline which is required for the text to
    appear
    (data (i32.const 8) "hello world\n")

    (func $main (export "_start")
        ;; Creating a new io vector within linear memory
        ;; iov.iov_base - This is a pointer to the start of the
        ;; 'hello world\n' string
        (i32.store (i32.const 0) (i32.const 8))
        ;; iov.iov_len - The length of the 'hello world\n' string
        (i32.store (i32.const 4) (i32.const 12))

        (call $fd_write
            (i32.const 1) ;; file_descriptor - 1 for stdout
            (i32.const 0) ;; *iovs - The pointer to the iov array at
            memory loc 0
            (i32.const 1) ;; iovs_len - We're printing 1 string
            (i32.const 20) ;; nwritten - Mem loc to store the # of bytes
```
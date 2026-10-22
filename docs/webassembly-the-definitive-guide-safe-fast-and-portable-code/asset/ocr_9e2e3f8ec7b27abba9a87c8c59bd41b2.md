We saw in Chapter 10 that we can use the WebAssembly backend to generate modules from Rust, but with all of the limitations involving strings, memory-management, interacting with the operating system, etc. it does not work.

```
brian@tweezer ~/g/w/s/c/hello-world> cargo build ↵
  --target wasm32-unknown-unknown --release
    Compiling hello-world v0.1.0 (/Users/brian/git-personal/webassembly-the-definitive-guide/src/ch11/hello-world)
    Finished release [optimized] target(s) in 0.89s
brian@tweezer ~/g/w/s/c/hello-world> ls -laF target/wasm32-unknown-unknown/release/
total 3008
drwxr-xr-x  10 brian  staff   320 Jun 27 16:02 ./
drwxr-xr-x@  5 brian  staff   160 Jun 27 16:02 ../
drwxr-xr-x   2 brian  staff    64 Jun 27 16:02 build/
drwxr-xr-x   4 brian  staff   128 Jun 27 16:02 deps/
drwxr-xr-x   2 brian  staff    64 Jun 27 16:02 examples/
-rw-r--r--   1 brian  staff   228 Jun 27 16:02 hello-world.d
-rwxr-xr-x   2 brian  staff 1534049 Jun 27 16:02 hello-world.wasm*
drwxr-xr-x   2 brian  staff    64 Jun 27 16:02 incremental/
brian@tweezer ~/g/w/s/c/hello-world > wasm3 target/wasm32-unknown-unknown/release/hello-world.wasm
Error: [Fatal] repl_call: function lookup failed
Error: function lookup failed ('_start')
```

A fundamental problem is that the basic module structure does not define the same type of Application Binary Interface (ABI)³ that an executable program does. It does not have the expected intialization function name. Developers think that main() is the starting point, but that is generally what is called by the very minimal C runtime environment from a method called "start".

Another issue is that there is no direct way to write to the console or read and write files from a WebAssembly module. This would seem to be a pretty big obstacle for WebAssembly, but I encourage you not to give up hope just yet! What happens if we use a different backend? Rather than wasm32-unknown-unknown we will use wasm32-wasi.
```
brian@tweezer ~/g/w/s/c/hello-world> cargo build --target wasm32-wasi --release
    Compiling hello-world v0.1.0 (/Users/brian/git-personal/webassembly-the-definitive-guide/src/ch11/hello-world)
    Finished release [optimized] target(s) in 0.74s
brian@tweezer ~/g/w/s/c/hello-world> wasm3 target/wasm32-wasi/release/hello-world.wasm
Hello, world!
```

Huh. Will you look at that?

Let’s investigate the module.

```
brian@tweezer ~/g/w/s/c/hello-world > wasm-objdump -x target/wasm32-wasi/release/hello-world.wasm
hello-world.wasm:          file format wasm 0x1

Section Details:

Type[18]:
  - type[0] () -> nil
  - type[1] (i32) -> nil
  - type[2] (i32) -> i64
  - type[3] (i32, i32) -> nil
  - type[4] (i32, i32) -> i32
  - type[5] (i32, i32) -> i64
  - type[6] (i32) -> i32
  - type[7] (i32, i32, i32) -> i32
  - type[8] (i32, i32, i32, i32) -> i32
  - type[9] () -> i32

...
Import[4]:
  - func[0] sig=8 <_ZN4wasi13lib_generated22wasi_snapshot_preview1_8fd_write> <- wasi_snapshot_preview1.fd_write
  - func[1] sig=1 <__wasi_proc_exit> <- wasi_snapshot_preview1.proc_exit
  - func[2] sig=4 <__wasi_environ_sizes_get> <- wasi_snapshot_preview1.environ_sizes_get
  - func[3] sig=4 <__wasi_environ_get> <- wasi_snapshot_preview1.environ_get
...

Export[5]:
  - memory[0] -> "memory"
```
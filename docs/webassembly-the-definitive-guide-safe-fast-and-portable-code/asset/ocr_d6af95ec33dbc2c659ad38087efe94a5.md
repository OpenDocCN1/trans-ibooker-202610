is easy to install multiple channels whenever you want. It is also possible to install backends for other architectures if you are interested in cross-compiling.

Start by installing the rustup toolchain documented here:

https://www.rust-lang.org/tools/install

To generate WebAssembly directly you will have to install the backend. You can pick whichever channel you want, but to install the nightly WebAssembly backend you would do this:

```
~/s/r/wasm> rustup target add wasm32-unknown-unknown --toolchain nightly
```

To use the backend when compiling Rust, you would do something like this if you did not make it the default toolchain:

```
~/s/r/wasm> rustc +nightly --target wasm32-unknown-unknown \
  -O --crate-type=cdylib add.rs -o add.wasm
```

To generate WASI code, you will want the WASI backend installed:

```
~/s/r/wasm> rustup target add wasm32-wasi --toolchain nightly
```

To use it if it is not the default toolchain:

```
~/s/r/wasm> rustc hello.rs --target wasm32-wasi
```

Notice that the regular Rust native backend, regular WebAssembly and WASI backends are distinct depending on what you want to target.

If you want to interact seamlessly between Rust and JavaScript, you will probably want to install wasm-bindgen.

An excellent introduction to the tool is here:

https://rustwasm.github.io/wasm-bindgen/

With the Rust tools installed, you should be able to install it with:
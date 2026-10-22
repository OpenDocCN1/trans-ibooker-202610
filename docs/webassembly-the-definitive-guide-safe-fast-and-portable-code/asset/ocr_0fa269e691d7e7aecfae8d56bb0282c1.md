![](bbox=[0.18, 0.11, 0.82, 0.31])

Figure 10-1. Invoking Rust from HTML

If that was all we had to talk about with Rust and WebAssembly, that would not be all that exciting. Fortunately, thanks to wasm-bindgen things get much more interesting very quickly.

# wasm-bindgen

In subsequent chapters, I will introduce you to several features that will be unlocked by proposals drafted post Minimum Viable Product (MVP). This includes the ability to reference more complex structures such as strings and lists, threading support, multi-value return types and more. Until then, wasm-bindgen has been a bigger help in bridging JavaScript and Rust at a high level so that you can pass data across the chasm beyond just numbers. The tool is not intended to be solely for Rust, but so far that has been where most of the benefits have been seen.

If you have installed wasm-bindgen and wasm-pack as described in Appendix A you should have everything you need for the remainder of the chapter. The latter is not a requirement but it makes things easier so we are going to start by using its bundling capabilities.

The “Hello, World!” for wasm-bindgen is invoking the alert() JavaScript method from Rust without having to import a method directly in